---
title: "하나의 Kubernetes 명령어가 초래한 비극"
description: ""
coverImage: "/assets/img/2024-07-09-ATragedyCausedbyaSingleKubernetesCommand_0.png"
date: 2024-07-09 22:55
ogImage: 
  url: /assets/img/2024-07-09-ATragedyCausedbyaSingleKubernetesCommand_0.png
tag: Tech
originalTitle: "A Tragedy Caused by a Single Kubernetes Command"
link: "https://medium.com/@zouyee/a-tragedy-caused-by-a-single-kubernetes-command-7b6126b06513"
---


# 설명

CentOS의 EOL로 인해 작년에는 내부적으로 새 OS로 이주하는 데 바빴습니다. 우리는 cgroup v1에서 cgroup v2로 전환하기로 결정했습니다. 그러나 구 cgroupv2에 대응하는 과정에서 일부 문제가 발생했습니다. 처음에는 이전 버전의 Kubernetes에서 cgroupv2에 적응하는 과정에서 문제가 발생했습니다. 처음에는 kubelet이 cgroup v1 환경에서 -enable_load_reader를 사용하여 컨테이너 CPU 부하 및 기타 관련 모니터링 데이터를 노출했습니다. 그러나 cgroup v2 환경에서 이 구성을 사용하면 kubelet이 패닉 상태가 되었습니다.

아래는 주요 세부 정보입니다:

```js
container.go:422] "/kubepods.slice/kubepods-besteffort.slice/kubepods-besteffort-podXXX.slice"의 cpu 로드 리더를 초기화할 수 없음: netlink를 기반으로 한 cpuload 리더를 만들지 못했습니다: 작업 통계를 위한 Netlink 패밀리 ID를 가져 오지 못했습니다: binary.Read: 잘못된 유형 int32
```

<div class="content-ad"></div>

# 기술적 배경

이 섹션에서는 다음 주제를 다룹니다:

- 컨테이너 메트릭을 생성하는 방법
- Kubernetes가 컨테이너 모니터링을 통합하는 방법
- CPU 부하가 계산되는 방법

# cadvisor

<div class="content-ad"></div>

cAdvisor은 컨테이너 환경에 특별히 설계된 강력한 Docker 컨테이너 모니터링 도구로, 자원 사용량 및 성능 분석을 용이하게 합니다. 컨테이너에 관한 relevant 정보를 수집, 집계, 처리하고 출력하는 데 사용됩니다. cAdvisor는 Docker 컨테이너 및 다른 유형의 컨테이너 실행 환경을 지원합니다.

Kubelet은 cAdvisor에 대한 내장 지원을 갖고 있어 사용자가 Kubelet 구성 요소를 통해 노드의 컨테이너에 대한 모니터링 메트릭을 직접 얻을 수 있습니다.

` K8s 1.19에서 cAdvisor 버전 0.39.3을 사용하고 있으며, 여기에서 간단히 소개하는 내용은 버전 0.48.1을 사용하고 있습니다.

아래는 주요 기능 코드이며, 가독성을 높이기 위한 주석이 포함되어 있습니다. 코드 경로는 다음과 같습니다: /cadvisor/cmd/cadvisor.go.

<div class="content-ad"></div>

cAdvisor는 주로 다음과 같은 작업을 수행합니다:

- Prometheus 인터페이스와 일반 API 인터페이스를 포함한 외부 사용을 위한 외부 API를 제공합니다.
- BigQuery, Elasticsearch, InfluxDB, Kafka, Prometheus, Redis, StatsD 및 표준 출력과 같은 제삼자 데이터 저장을 지원합니다.
- 컨테이너, 프로세스, 머신, Go 런타임 및 사용자 정의 비즈니스 로직에 관련된 모니터링 데이터를 수집합니다.

```js
func init() {
    optstr := container.AllMetrics.String()
    flag.Var(&ignoreMetrics, "disable_metrics", fmt.Sprintf("비활성화할 메트릭의 쉼표로 구분된 목록입니다. 옵션은 %s입니다.", optstr))
    flag.Var(&enableMetrics, "enable_metrics", fmt.Sprintf("활성화할 메트릭의 쉼표로 구분된 목록입니다. 설정된 경우 'disable_metrics'를 재정의합니다. 옵션은 %s입니다.", optstr))
}
```

위 코드에서 볼 수 있듯이 cAdvisor는 특정 메트릭을 활성화 또는 비활성화하는 기능을 지원합니다. 그 중에서도 AllMetrics에는 주로 다음과 같은 메트릭이 포함되어 있습니다:

<div class="content-ad"></div>


[factory.go의 해당 코드](https://github.com/google/cadvisor/blob/master/container/factory.go#L72)

```js
var AllMetrics = MetricSet{
    CpuUsageMetrics:                struct{}{},
    ProcessSchedulerMetrics:        struct{}{},
    PerCpuUsageMetrics:             struct{}{},
    MemoryUsageMetrics:             struct{}{},
    MemoryNumaMetrics:              struct{}{},
    CpuLoadMetrics:                 struct{}{},
    DiskIOMetrics:                  struct{}{},
    DiskUsageMetrics:               struct{}{},
    NetworkUsageMetrics:            struct{}{},
    NetworkTcpUsageMetrics:         struct{}{},
    NetworkAdvancedTcpUsageMetrics: struct{}{},
    NetworkUdpUsageMetrics:         struct{}{},
    ProcessMetrics:                 struct{}{},
    AppMetrics:                     struct{}{},
    HugetlbUsageMetrics:            struct{}{},
    PerfMetrics:                    struct{}{},
    ReferencedMemoryMetrics:        struct{}{},
    CPUTopologyMetrics:             struct{}{},
    ResctrlMetrics:                 struct{}{},
    CPUSetMetrics:                  struct{}{},
    OOMMetrics:                     struct{}{},
}
```

```js
func main() {
    ...

    var includedMetrics container.MetricSet
    if len(enableMetrics) > 0 {
        includedMetrics = enableMetrics
    } else {
        includedMetrics = container.AllMetrics.Difference(ignoreMetrics)
    }
    
    klog.V(1).Infof("enabled metrics: %s", includedMetrics.String())
    setMaxProcs()
    
    memoryStorage, err := NewMemoryStorage()
    if err != nil {
        klog.Fatalf("저장 드라이버를 초기화하는 데 실패했습니다: %s", err)
    }

    sysFs := sysfs.NewRealSysFs()

    
    // cadvisor의 핵심
    resourceManager, err := manager.New(memoryStorage, sysFs, manager.HousekeepingConfigFlags, includedMetrics, &collectorHTTPClient, strings.Split(*rawCgroupPrefixWhiteList, ","), strings.Split(*envMetadataWhiteList, ","), *perfEvents, *resctrlInterval)
    if err != nil {
        klog.Fatalf("매니저 생성에 실패했습니다: %s", err)
    }


    // registry http handler
    err = cadvisorhttp.RegisterHandlers(mux, resourceManager, *httpAuthFile, *httpAuthRealm, *httpDigestFile, *httpDigestRealm, *urlBasePrefix)
    if err != nil {
        klog.Fatalf("HTTP 핸들러 등록에 실패했습니다: %v", err)
    }
    // 컨테이너 라벨 커스터마이즈 kubelet 1.28에서 CRI로 변경하는 경우 kubelet 수정이 필요합니다
    containerLabelFunc := metrics.DefaultContainerLabels
    if !*storeContainerLabels {
        whitelistedLabels := strings.Split(*whitelistedContainerLabels, ",")
        // 라벨의 공백 제거
        for i := range whitelistedLabels {
            whitelistedLabels[i] = strings.TrimSpace(whitelistedLabels[i])
        }
        containerLabelFunc = metrics.BaseContainerLabels(whitelistedLabels)
    }

    ...
}
```

CPU 부하 메트릭 생성은 enable_load_reader 커맨드 라인 플래그로 제어됩니다.


[container.go의 해당 코드](https://github.com/google/cadvisor/blob/42bb3d13a0cf9ab80c880a16c4ebb4f36e51b0c9/manager/container.go#L455)

```js
if *enableLoadReader {
        // CPU 부하 리더 생성.
        loadReader, err := cpuload.New()
        if err != nil {
            klog.Warningf("%q을 위한 CPU 부하 리더를 초기화할 수 없습니다: %s", ref.Name, err)
        } else {
            cont.loadReader = loadReader
        }
    }
```

<div class="content-ad"></div>

# 쿠블릿

Kubernetes에서는 구글의 cAdvisor 프로젝트가 노드에서 컨테이너 자원 및 성능 지표를 수집하기 위해 사용됩니다. 쿠블릿 서버 내에서 cAdvisor는 해당 노드의 kubepods(기본 cgroup 이름인 “.slice” 접미사가 있는 systemd 모드 하에) cgroup 하의 모든 컨테이너를 모니터링하기 위해 통합되어 있습니다. 1.29.0-alpha.2 버전을 기준으로 쿠블릿은 아래 두 가지 구성 옵션을 현재 제공하고 있습니다.(하지만 이제 useLegacyCadvisorStats가 false로 설정되어 있습니다):

```js
if kubeDeps.useLegacyCadvisorStats {
    klet.StatsProvider = stats.NewCadvisorStatsProvider(
      klet.cadvisor,
      klet.resourceAnalyzer,
      klet.podManager,
      klet.runtimeCache,
      klet.containerRuntime,
      klet.statusManager,
      hostStatsProvider)
  } else {
    klet.StatsProvider = stats.NewCRIStatsProvider(
      klet.cadvisor,
      klet.resourceAnalyzer,
      klet.podManager,
      klet.runtimeCache,
      kubeDeps.RemoteRuntimeService,
      kubeDeps.RemoteImageService,
      hostStatsProvider,
      utilfeature.DefaultFeatureGate.Enabled(features.PodAndContainerStatsFromCRI))
  }
```

쿠블릿은 Prometheus 메트릭 형식으로 모든 관련 런타임 지표를 /stats/에서 공개합니다. 아래 도표에 나와 있는 것처럼 쿠블릿은 cAdvisor 서비스를 내장하고 있습니다.

<div class="content-ad"></div>


<img src="/assets/img/2024-07-09-ATragedyCausedbyaSingleKubernetesCommand_0.png" />

마침내, 우리는 kubelet 내에서 cAdvisor 구성 요소가 어떻게 초기화되는지 볼 수 있습니다.

```js
https://github.com/kubernetes/kubernetes/blob/master/pkg/kubelet/cadvisor/cadvisor_linux.go#L80

func New(imageFsInfoProvider ImageFsInfoProvider, rootPath string, cgroupRoots []string, usingLegacyStats, localStorageCapacityIsolation bool) (Interface, error) {
    sysFs := sysfs.NewRealSysFs()
    // 여기는 kubelet에 의해 기본적으로 노출되는 모니터링 지표 유형입니다.
    includedMetrics := cadvisormetrics.MetricSet{
        ...
        cadvisormetrics.CpuLoadMetrics:      struct{}{},
        ...
    }
    // cAdvisor 컨테이너 관리자 초기화.
    m, err := manager.New(memory.New(statsCacheDuration, nil), sysFs, housekeepingConfig, includedMetrics, http.DefaultClient, cgroupRoots, nil /* containerEnvMetadataWhiteList */, "" /* perfEventsFile */, time.Duration(0) /*resctrlInterval*/)
    ...
```

여기는 cAdvisor를 위한 manager.New 함수 인터페이스의 직접 호출입니다. 더 자세한 정보는 다음을 참조하세요: https://zoues.com/posts/3f237e52/


<div class="content-ad"></div>

# CPU 부하 지표

CPU 사용량은 현재 CPU 활동 수준을 나타내며 CPU 부하 평균은 일정 기간 내에서 CPU를 활성적으로 사용하는 프로세스 및 CPU 시간을 기다리는 프로세스의 수를 나타냅니다. 여기서 CPU 시간을 기다리는 프로세스는 깨어나기를 기다리는 프로세스로, 대기 상태의 프로세스는 제외됩니다.

장치를 진단할 때 CPU 사용량, 부하 평균 및 작업 상태를 결합하여 판단하는 것이 필요합니다. 예를 들어, CPU 사용량이 낮지만 부하 평균이 높으면 I/O 병목 현상을 나타낼 수 있지만, 여기서는 그에 대해 자세히 다루지 않겠습니다.

cAdvisor에서 노출하는 메트릭 이름은 다음과 같습니다:

<div class="content-ad"></div>

```js
container_cpu_load_average_10s
```

이제 계산 방법을 살펴보겠습니다.

```js
https://github.com/google/cadvisor/blob/master/manager/container.go#L632

// 새로운 실행 가능한 스레드 샘플을 사용하여 새로운 부드러운 부하 평균을 계산합니다.
// 사용된 감쇠는 부하가 10초 이내에 새로운 상수값으로 안정화되도록합니다.
func (cd *containerData) updateLoad(newLoad uint64) {
    if cd.loadAvg < 0 {
        cd.loadAvg = float64(newLoad) // 빠른 안정화를 위해 첫 번째 본 샘플을 초기화합니다.
    } else {
        cd.loadAvg = cd.loadAvg*cd.loadDecay + float64(newLoad)*(1.0-cd.loadDecay)
    }
}
```

계산 방법은 다음과 같습니다: cd.loadAvg = cd.loadAvg*cd.loadDecay + float64(newLoad)*(1.0-cd.loadDecay)

<div class="content-ad"></div>

이전 수집 값인 cd.loadAvg에 있는 값을 최근 수집된 값인 newLoad에 (1.0-cd.loadDecay)를 곱하여 더한 후, cd.loadDecay로 곱한 값으로 곱해야합니다.

여기 cont.loadDecay를 계산하는 로직이 있습니다:

```js
https://github.com/google/cadvisor/blob/master/manager/container.go#L453

cont.loadDecay = math.Exp(float64(-cont.housekeepingInterval.Seconds() / 10))
```

housekeepingInterval과 관련된 decay window라고 알려진 고정값이 있습니다.

<div class="content-ad"></div>

# 소스로 거슬러 올라가기

이전 cd.loadAvg의 CPU 부하 값은 다음과 같이 얻어집니다:

```js
https://github.com/google/cadvisor/blob/master/manager/container.go#L650

if cd.loadReader != nil {
        // TODO(vmarmol): 이 경로를 캐시하십시오.
        path, err := cd.handler.GetCgroupPath("cpu")
        if err == nil {
            loadStats, err := cd.loadReader.GetCpuLoad(cd.info.Name, path)
            if err != nil {
                return fmt.Errorf("%q - 경로 %q에 대한 부하 통계를 가져오는 데 실패했습니다. 오류: %s", cd.info.Name, path, err)
            }
            stats.TaskStats = loadStats
            cd.updateLoad(loadStats.NrRunning)
            // 부동 소수점을 피하고 정밀도를 유지하려면 'milliLoad'로 변환합니다.
            stats.Cpu.LoadAverage = int32(cd.loadAvg * 1000)
        }
    }
```

더 깊이 파고들면, netlink가 시스템 메트릭을 검색하기 위해 사용된다는 것을 발견할 수 있습니다. 중요한 호출 경로는 다음과 같습니다:

<div class="content-ad"></div>

위의 분석을 통해, cAdvisor는 CGROUPSTATS_CMD_GET 요청을 보내고 netlink 메시지를 통해 통신하여 CPU 부하 정보를 가져옵니다.

```js
cadvisor/utils/cpuload/netlink/netlink.go
```

v0.48.1 브랜치의 128~132번 라인에서 확인할 수 있습니다.

<div class="content-ad"></div>

```go
func prepareCmdMessage(id uint16, cfd uintptr) (msg netlinkMessage) {
    buf := bytes.NewBuffer([]byte{})
    addAttribute(buf, unix.CGROUPSTATS_CMD_ATTR_FD, uint32(cfd), 4)
    return prepareMessage(id, unix.CGROUPSTATS_CMD_GET, buf.Bytes())
}
```

마지막으로, 커널은 cgroupstats_user_cmd에서 검색 요청을 처리합니다:

```c
/* user->kernel request/get-response */
```

kernel/taskstats.c#L407


<div class="content-ad"></div>

```js
정적 int cgroupstats_user_cmd(struct sk_buff *skb, struct genl_info *info)
{
    int rc = 0;
    struct sk_buff *rep_skb;
    struct cgroupstats *stats;
    struct nlattr *na;
    size_t size;
    u32 fd;
    struct fd f;

    na = info->attrs[CGROUPSTATS_CMD_ATTR_FD];
    if (!na)
        return -EINVAL;

    fd = nla_get_u32(info->attrs[CGROUPSTATS_CMD_ATTR_FD]);
    f = fdget(fd);
    if (!f.file)
        return 0;

    size = nla_total_size(sizeof(struct cgroupstats));

    rc = prepare_reply(info, CGROUPSTATS_CMD_NEW, &rep_skb,
                size);
    if (rc < 0)
        goto err;

    na = nla_reserve(rep_skb, CGROUPSTATS_TYPE_CGROUP_STATS,
                sizeof(struct cgroupstats));
    if (na == NULL) {
        nlmsg_free(rep_skb);
        rc = -EMSGSIZE;
        goto err;
    }

    stats = nla_data(na);
    memset(stats, 0, sizeof(*stats));

    rc = cgroupstats_build(stats, f.file->f_path.dentry);
    if (rc < 0) {
        nlmsg_free(rep_skb);
        goto err;
    }

    rc = send_reply(rep_skb, info);

err:
    fdput(f);
    return rc;
}
```

그리고 cgroupstats_build 함수에서 cgroup stats 결과를 구성합니다:

kernel/cgroup/cgroup-v1.c#L699

```js
/**
 * cgroupstats_build - cgroupstats를 빌드하고 채웁니다
 * @stats: 정보를 채울 cgroupstats
 * @dentry: 요청된 통계가 있는 cgroup에 속한 dentry 항목
 *
 * cgroupstats를 빌드하고 채워서 taskstats가 사용자 공간으로 내보내도록 합니다.
 *
 * 성공 시 %0을 반환하거나 실패 시 음수 errno 코드를 반환합니다
 */
int cgroupstats_build(struct cgroupstats *stats, struct dentry *dentry)
{
……
    /* cgroupfs에 속하는 kernfs_node이어야 하며 디렉토리여야 합니다 */
    if (dentry->d_sb->s_type != &cgroup_fs_type || !kn ||
        kernfs_type(kn) != KERNFS_DIR)
        return -EINVAL;
```

<div class="content-ad"></div>

여기서 cgroup_fs_type이 cgroup v1의 유형인 것을 알 수 있으며, cgroup v2를 처리하는 부분이 없습니다. 따라서 cgroupstats_build 함수는 경로 유형 판단문에서 EINVAL을 반환합니다.

커널 커뮤니티에서도 이 문제에 대한 설명이 있습니다: [커널 커뮤니티 이슈](kernel community issue)

Tejun (메타, cgroupv2 소유자)가 이에 대해 설명하는 방법을 살펴보겠습니다:

cgroupstats 작업을 v2 인터페이스에서 의도적으로 제외한 이유는 남은 통계 데이터와 중복되고 일관성이 없기 때문입니다.

<div class="content-ad"></div>

# 결론

따라서 그의 제안은 무엇인가요?

그는 CGROUPSTATS_CMD_GET 넷링크 API를 통해 CPU 통계 정보를 가져오는 대신 PSI를 사용하는 것을 제안했습니다. 대신 cpu.pressure, memory.pressure 및 io.pressure 파일에서 직접 정보를 가져와야 합니다. 후에 컨테이너 분야에서 PSI의 관련 진전에 대해 논의할 것입니다. 현재 Containerd는 이미 PSI 관련 모니터링을 지원하고 있습니다.

# 참고문헌

<div class="content-ad"></div>

https://github.com/containerd/cgroups/pull/308

https://cloud.tencent.com/developer/article/2329489

https://github.com/google/cadvisor/issues/3137

https://www.cnblogs.com/vinsent/p/15830271.html

<div class="content-ad"></div>

https://zoues.com/posts/5a8a6c8d/