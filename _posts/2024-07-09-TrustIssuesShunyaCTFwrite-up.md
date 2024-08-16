---
title: "ShunyaCTF Trust Issues 문제 풀이 및 해설"
description: ""
coverImage: "/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_0.png"
date: 2024-07-09 09:57
ogImage: 
  url: /assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_0.png
tag: Tech
originalTitle: "Trust Issues ShunyaCTF write-up"
link: "https://medium.com/@tavilefty/trust-issues-shunyactf-write-up-2c38ece7aa33"
---


지난 주 nCreeps에서 첫 번째 오프라인 CTF인 ShunyaCTF를 주최했는데, 이 이벤트의 주최자로써 이벤트를 위해 CTF 도전 과제를 만들 기회가 있었어요. 그래서 제가 만든 도전 과제 중 하나인 Trust Issues에 대해 이야기할게요. 이는 허니팟(스포일러), 함정 그리고 신뢰 문제를 기반으로 한 리눅스 박스예요 😋

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_0.png)

방 링크: [여기를 클릭해주세요](https://tryhackme.com/r/room/trustissuesshunyactf)

# 포괄적인 해법 방법

<div class="content-ad"></div>

도전 과제를 몇 가지 부분으로 나눠볼게요:

- 도전 과제/설명 이해하기
- 시스템과 상호작용하기 (탐색)
- 진입하기
- 권한 상승

## 설명 이해하기

음... 그러면, 대상은 aalu이고 ssh 브루트 포싱과 관련이 있다네요. 비밀번호 목록은 munged password에 대한 위키피디아 문서에서 가져와야 합니다. 'rock him' 부분은 아마 rockyou.txt의 사용을 제안하는 것 같네요?

<div class="content-ad"></div>

## 시스템과 상호작용하기

박스를 시작하고 첫 번째 작업을 실행해봅시다 — nmap 포트 스캔.

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_1.png)

sudo를 왜 사용해야 하나요?

<div class="content-ad"></div>

네, nmap은 기본적으로 대상에 대해 TCP 스캔을 수행하며 TCP는 3-way 핸드셰이크를 필요로 합니다. 이에 우리의 친절한 nmap은 다음 포트로 넘어가기 전에 3-way 핸드셰이크를 완료할 것을 확실히 합니다. 그러나 핸드셰이크를 완료하는 것은 좋은 제스처이지만 (ACK 패킷을 기다리는 포트를 방치하지 않게 됩니다) 많은 시간이 걸립니다. 이는 대회에서 원치 않을 일이죠!

그 이유를 이해하게 될 거에요.

포트 22가 열려 있고 nmap은 이것이 ssh 서비스라고 말하고 있습니다. 이제 포트 22에 대해 공격적인 스캔을 해봅시다:

<div class="content-ad"></div>

적극적인 스캔은 다음을 수행합니다:

- 서비스 버전/배너 정보 수집 (-sV)
- 기본 스크립트 실행 (-sC)
- IP의 운영 체제 찾기 (-O)
- 전송되는 패킷 경로 찾기 (--traceroute)

우리는 OpenSSH 6.0p1 Debian 4+deb7u2 버전을 확인했고, 구글에서 취약점을 확인했지만 나중에 살펴보겠습니다. 설명은 주로 암호 해독에 관련되어 있습니다.

아마 이 글이 aalu의 ssh의 열쇠일 거에요? (이 쿼리로 얻은 것인데, 아주 테크니컬하죠, 그렇죠?) 이제 이 글을 스크랩해야 하며, 챌린지에 "임의의 단어를 비밀번호로 선택했다"고 되어 있습니다 (적어도 8자리이며 123이 추가된 총 11자/11자 이상이 되도록) 우리는 8자리 길이의 단어를 확실히 스크랩해야 합니다.

<div class="content-ad"></div>

해당하는 Markdown 양식으로 표를 변경할 수 있습니다.


document.designMode = "on";
document.execCommand("selectAll");


이렇게 작성하면 기사의 모든 텍스트가 선택됩니다. 또한 cmd+A, cmd+C도 사용할 수 있어요.

CeWL을 사용했어요.

<div class="content-ad"></div>

다음 쿼리를 실행했습니다:

cewl -d0 -m8 https://en.wikipedia.org/wiki/Munged_password -w aalu.txt

이 명령은 cewl이 깊이를 0으로 설정하여 (-d0, 따라서 링크가 따라가지 않음), 최소 길이를 8로 설정하고 (-m8), 그리고 단어를 aalu.txt에 출력하도록 지시합니다.

작은 파이썬 스크립트를 실행했습니다:

<div class="content-ad"></div>

```python
# 단어 목록에 각 단어에 '123'을 추가하는 함수
def append_suffix_to_wordlist(input_file, output_file, suffix='123'):
    try:
        # 입력 파일에서 단어 목록을 읽기
        with open(input_file, 'r') as infile:
            words = infile.readlines()

        # 각 단어에 접미사를 추가하여 새 목록에 저장
        modified_words = [word.strip() + suffix + '\n' for word in words]

        # 수정된 단어를 출력 파일에 쓰기
        with open(output_file, 'w') as outfile:
            outfile.writelines(modified_words)

        print(f"'{suffix}'를 각 단어에 추가하여 {input_file}의 각 단어에 성공적으로 저장되었습니다: {output_file}.")
    except Exception as e:
        print(f"단어 목록 처리 중 오류 발생: {e}")

# 입력 단어 목록 파일 경로
input_file = 'wordlist.txt'

# 출력 단어 목록 파일 경로
output_file = 'modified_wordlist.txt'

# 단어 목록 처리 함수 호출
append_suffix_to_wordlist(input_file, output_file)
```

그리고 우리의 패스리스트가 있습니다. SSH 서버에서 실행했는데 (자동화 도구로 실행하면 됩니다. 저는 개인적으로 metasploit의 ssh_login을 사용했고, 어떤 플레이어들은 hydra를 사용했습니다) 작동하지 않았어요. 흠...

<img src="/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_3.png" />

제 단어 목록이 잘못된 것일까요? 아마 그렇지 않겠죠? 왜냐하면 CeWL은 위키피디아(크롤링 허용)에서 잘 작동합니다. 심지어 js 콘솔 방법을 사용해도 작동하지 않아요. 뭔가 빠트린 게 있을 것 같네요!


<div class="content-ad"></div>

노트: 사용자들은 스크립트도 사용할 수 있어요. 위키피디아의 파이썬 라이브러리를 사용한 이 기사는 정말 멋지네요.

지금은 두 가지 가능성이 있어요:

- rockyou를 사용해볼까? (go rock him이 암시했다.)
- 더 많은 탐색을 해볼까?

1번 옵션이 믿음의 도약처럼 보인다면, 2번 단계를 따라해봐요 (이건 사실, 도전의 원래 작동 방식이에요).

<div class="content-ad"></div>

우리는 무언가 흥미로운 것을 찾기 위해 전체 nmap 스캔을 시도해 봐요 (다른 방법이 뭘까요?)

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_4.png)

여기서 half-handshake가 우리를 정말 도와줍니다. 더불어 우리는 속도를 T5로 설정했어요 (매우 빠름).

가끔 nmap이 놓치는 일이 있으니 2-3번 스캔을 하는 것을 제안해요, 어라, nmap은 이제 우리에게 불신을 줘? :(

<div class="content-ad"></div>

nmap에서 때때로 놓치는 이유가 뭘까요? nmap은 서버로 패킷을 보내고 일정 시간 동안 대기한 다음, 해당 시간 내에 패킷이 도착하지 않으면 포트를 필터링하거나 닫힌 것으로 표시합니다. 그래서 주로 도구에 의존해서는 안 되고, 의존하게 된다면 그 도구에 대해 꽤 익숙해져야 합니다.

다음과 같은 것을 할 수 있어요:
- --max-retries(기본값 10): 포기하기 전에 포트로 패킷을 보낼 때 몇 번 시도할지 정의합니다.
- --min-rate: 스캔 속도의 기준을 설정합니다. 너무 빠르면 실제로 시스템을 DoS할 수 있습니다. 비슷하게 --max-rate는 한 번에 보내는 패킷의 최대 제한을 설정합니다. 레거시 시스템에서 작업 중이라면 이러한 값을 적절히 조정해주세요.

이제 nmap이 열려 있는 포트를 닫혀 있다고 표시할 수 있는 이유를 이해했어요.

<div class="content-ad"></div>

포트 420에서 서비스를 스캔하고 확인했어요, smpte야. 구글링해보니 더 이상 사용되지 않는다고 하네요. 괜찮아요, 그럼 이 포트와 어떻게 상호작용할까요? 아마도 netcat을 통해 배너 그래빙을 해볼까요?

<img src="/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_5.png" />

기다려요, 이게 smtpe 서비스인가요 아니면 ssh인가요? 왜 다른 결과를 받는 걸까요?

nmap은 각 포트를 서비스에 매핑하고, 오픈된 포트를 보면 해당 포트에 매핑된 서비스만 보여주죠. 제발, nmap! 믿음문제를 일으키고 있어요😔

<div class="content-ad"></div>

앞으로 나아가기 전에 포트 22가 무엇인지 확인해 봅시다.

![image](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_6.png)

첫 번째 옵션을 선택하고 22 포트 ssh에 rockyou.txt를 사용하여 공격을 설정한 후 실행해 봅시다.

![image](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_7.png)

<div class="content-ad"></div>

아우 너! TCP/22번 포트의 ssh 암호는 'fuckyou' (매우 무례한 암호)야.

## 들어가보기

우리는 ssh에 접속해서 주변을 둘러보려고 해. 이게 허니팟이니까, 이 곳은 공격자들이 열받게 만드는 일을 하는데, 동시에 그 흔적도 남기려고 하지. 네가 여기저기 둘러보고 자유롭게 시도해볼 수 있어. /proc 디렉토리로 들어가보면 (리눅스 시스템에서 가상이며 파일 크기가 없지만 cat으로 볼 수 있는 내용을 가지고 있음) 내용을 cat하려고 하면 안 돼. stat, file과 같은 명령어도 찾을 수 없을 거야, 이건 컨테이너와 같은 동작을 한다고 볼 수 있어. (사실상 허니팟 컨테이너지만 사용자에게는 단순한 컨테이너인 것처럼 보일 거야).

그리고 사용자는 자신이 속은 걸 깨달았고 이게 SSH 서버가 아닌 것을 알게 될 거야!

<div class="content-ad"></div>

네, 설명이 맞았네요! aalu.txt는 ssh 서버가 아니기 때문에 작동하지 않습니다. 시스템 어딘가에는 ssh 서버가 있습니다.

이제 여기서 조합에 접어들면, 포트 420을 스캔했더니, 다시 우리가 TCP/420을 banner grabbing하고 실제로 SSH임을 깨달았습니다.

버전은 이것입니다 — SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.11 이며 유효합니다. 이제 aalu.txt를 사용하여 브루트 포싱을 시도해봅시다.

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_8.png)

<div class="content-ad"></div>

그럼 시작해 볼까요! aalu의 SSH 비밀번호는 protection123 입니다.

## 권한 상승

여기는 TryHackMe 상자이니 인터넷에 액세스할 수 없지만 호스트를 브릿지로 사용하여 파일을 다운로드하고, 그 파일을 aalu의 SSH로 복사할 수 있습니다. 그 목적으로 파이썬 서버를 설정했습니다.

Linpeas를 사용하여 일부 특징이나 권한 상승 지표를 찾을 수 있습니다.

<div class="content-ad"></div>


<img src="/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_9.png" />

나는 더 세부적이고 수동적인 제어를 선호하기 때문에 다음과 같이 시작합니다.

find / -perm -u=s 2`/dev/null

이 명령은 루트 디렉토리에서 SUID 비트가 설정된 것을 찾아서 오류를 /dev/null에 넣을 것입니다.


<div class="content-ad"></div>

SUID 비트를 이해해 봅시다. 이는 권한 상승의 단순한 통로입니다.

높은 권한을 가진 사용자 A (하지만 root는 아님)가 러스트를 설치했고 어떤 이유로 인해 저 권한을 가진 사용자 B에게 그 이진 파일을 실행하도록 허용해야 할 경우, A는 러스트에 SUID를 설정할 수 있습니다. 이를 통해 누구든 그 이진 파일을 A로 실행할 수 있게 됩니다. A는 여전히 이진 파일을 소유하고 있지만 누구든 실행할 수 있으며, 실행뿐만 아니라 A로 실행할 수 있게 됩니다. 어디로 향하는지 보이시나요? B가 A로 러스트를 실행할 수 있다면, A가 할 수 있는 모든 작업을 할 수 있습니다. 이는 러스트가 허용하는 셸을 생성하고 A로 변신할 수 있다는 것을 의미합니다.

root 사용자가 아닌 사용자 A의 예제를 살펴본 이유는 종종 위 명령을 실행하면 사용자 루트 권한을 부여할 수 있는 이진 파일을 모두 나열할 것이라는 오해가 있기 때문입니다; 그러나 조직이나 실제 시나리오에서는 이렇게 작동하지 않습니다.

또한, '/dev/null`을 보면 이는 모든 오류를 의미합니다 (find가 aalu의 소유가 아닌 파일을 읽을 수 없어 오류가 발생한다고 볼 수 있으며 /dev/null로 이동합니다. 이는 아무것도 존재하지 않는 곳으로, 모든 입력을 소멸시킵니다. '1`은 오류가 아닌 표준 출력을 의미합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_10.png)

여기에 많은 이진 파일이 있어요. gtfobins를 사용해서 탈출할 수 있는 방법을 찾아봅시다.

음, 유용한 정보를 찾을 수 없네요. 그런데 여기 의심스러운 게 있네요. /usr/local/bin/womp은 무엇일까요?

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_11.png)


<div class="content-ad"></div>

아마도 이진수인 것 같아요. 실행해보죠:

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_12.png)

사실은 이것은 파이썬이었습니다.

os 라이브러리를 사용하여 사용자 ID(UID)를 0으로 설정했습니다. (womp 바이너리는 SUID가 설정되어 있어 0이 일반적으로 루트의 사용자 ID입니다. 그리고 이를 허용합니다.) 그런 다음 bash 쉘을 생성했습니다. 저는 중괄호 확장, 명령 치환 등을 지원하는 bash를 사용했습니다.

<div class="content-ad"></div>

우리들 진입 성공했어요!!

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_13.png)

플래그 획득할 시간입니다 🥰

/root 디렉토리로 이동해서 ls 명령어를 입력하면 400개 파일만 나오네요. 하지만 여기서 CTF 중이라면 ls만 한다고 끝일까요? NO! ls -a를 입력해 추가 파일들을 확인해봐요.

<div class="content-ad"></div>

ls -l 명령어를 사용하면 파일 크기에 대한 감을 얻을 수 있어요. 조금 일관성 없어 보이지만요.

음, 플래그는 shunyaCTF'...' 형식으로 되어 있다는 걸 알고 있으니 grep을 사용해볼까요?

```bash
grep -rl `shunya` . | xargs cat
```

위 명령어는 ‘shunya’ 단어를 재귀적으로 검색할 거에요. 그러면 shunyaCTF'...' 형식의 또 다른 임의의 단어 목록을 얻게 되고, 거기서 공통 단어 fakeflag을 볼 수 있어요. 이렇게 일치하는 패턴을 포함하지 않는 모든 단어를 제외할 수 있는 grep -v를 사용하면 좋겠죠?

```bash
<img src="/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_14.png" />
```

<div class="content-ad"></div>

우리가 깃발을 획득했어요! shunyaCTF'1_10v3_h0n3y'입니다.

# 추가 검사

이제 루트 액세스를 획득했으니, 포트 22에서 무엇이 실행 중이었는지 확인해봅시다.

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_15.png)

<div class="content-ad"></div>

제가 좋아하는 명령어 중 하나는 포트와 열린 파일을 확인하기 위한 lsof입니다. lsof -i :22 명령을 실행하면 포트 22에서 실행 중인 서비스를 확인할 수 있습니다. 보통 twistd인 것 같네요.

제가 그 명령에 grep을 적용했더니, 다시 한 번 user cowrie가 이 프로세스를 실행 중인 것을 보았습니다. 아마 PC에 aalu라는 사용자와 cowrie가 두 명 있어서, aalu가 420 포트에서 ssh를 실행하고 cowrie가 포트 22에서 실행하고 있는 것일까요? 그런데 그렇다면 cowrie의 ssh가 왜 aalu의 ssh인 것처럼 표시될까요? 이상하네요.

우리도 cowrie가 되어봅시다.

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_16.png)

<div class="content-ad"></div>

우리는 관심 있는 파일인 honeyfs와 etc/ (일반적으로 구성 파일)를 볼 수 있어요.

우리는 /etc로 이동해서 몇 가지 파일을 확인해 봅니다.

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_17.png)

userdb.txt 파일은 사용자 이름과 암호가 평문으로 나열된 파일이라는 것을 볼 수 있어요. 더불어, cowrie.cfg 파일에는 이러한 내용이 있습니다:

<div class="content-ad"></div>

[ssh]

listen_endpoints=tcp:22:interface=0.0.0.0
이것은 모든 IPv4에 대해 tcp/22에 실행될 것을 알려줍니다.

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_18.png)

이것은 /home/cowrie/cowrie/honeyfs/etc/shadow 파일이 보여주는 것이며, 이것은 22번 포트 ssh에서 shadow 파일 내용을 cat으로 확인했을 때 언뜻 보기에 비슷한 것이다.

<div class="content-ad"></div>

그리고 파일에는 이렇게 나와 있습니다... 이것은 허니팟이었어요!

![이미지](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_19.png)

## 허니팟이 뭔가요?

허니팟은 포트에서 실행되는 서비스로, 여기서는 SSH를 흉내내지만 텔넷이나 심지어 HTTP도 흉내낼 수 있습니다. 이들은 공격자에게 자신이 진짜 서버에서 하는 것처럼 착각하게 함으로써 실제 시스템을 복제합니다. 공격자의 각 단계를 캡처하고 나중에 다시 그에 대한 조치를 취할 수 있게 합니다.

<div class="content-ad"></div>

잼난 사실: FBI는 불법 서비스를 찾는 사람들을 유인하기 위해 꿀 덫 사이트를 사용해서 그들을 주시하고 있답니다.

카우리에 대해 구글링을 해보면

여기가 포인트인데요: 카우리는 알루라는 가짜 SSH를 실행하는 더미 계정이라는 이야기고, 실제 SSH는 420번 포트에서 실행돼요.

카우리를 분해해 보고 싶다면, /cowrie/src/cowrie/commands로 가서 그 명령들이 어떻게 작동하는지 확인할 수 있어요.

<div class="content-ad"></div>

아래는 요약입니다.
알림! 기술적 한계로 인해 몇 가지 교정이 필요합니다.
신속하게 확인하여 수정하겠습니다!

<div class="content-ad"></div>

nmap 이외에도 Rustscan 이라는 다른 포트 스캐닝 도구가 있어요. 이 도구는 nmap 보다 뛰어나고 빠르며, 잘못된 양성/음성 결과가 덜 발생해요. Rustscan 은 빠른 속도를 제공하는데, 이는 Rust 언어를 기반으로하기 때문이에요. 그러나 이 도구는 실제 시스템에 사용하기보다는 CTF(캡처 더 플래그)에 맞게 개발되었어요.

Rustscan 을 사용하려면 nmap, Rust 및 cargo 가 설치되어 있어야 하거나 도커 파일을 사용할 수 있어요!

구문은 rustscan -r 1-65535 -a `ip-addr` 이며, 몇 초 안에 모든 포트가 스캔되어, 제 도전의 가장 큰 부분이 무용지물이 됩니다.

## 권한 상승

<div class="content-ad"></div>

네 맞아요, 사실 womp 바이너리를 검사할 필요 없이 권한 상승을 할 수 있다고 하면 어떨까요?

저는 TryHackMe 박스에서 우분투 20.04 (Focal Fossa)를 선택했어요. 제가 몰랐던 사실이지만, 여기에는 보안 문제가 있더라구요. 기본 사용자가 lxd 그룹에 속해 있는 것이었죠.

lxd는 컨테이너를 실행해서 시스템 서비스를 제공하는 데 사용됩니다 (한편 도커와 같은 컨테이너 서비스는 일반적으로 응용 프로그램 서비스를 컨테이너화하는 데 사용됩니다).

lxd에 사용자가 속해 있는 것은 우분투 18.04에서 실제 취약점으로 나타났었지만, 우분투 20.04에서는 이게 문제가 될 것이라고 생각하지 않았어요. 하지만 그렇게 됐죠.

<div class="content-ad"></div>

일부 참가자들은 실제로 lxd를 사용하여 권한을 상승시켰어요.

# 상자의 목적

워크스루를 마치고 나니, 상자는 매우 쉽게 할 수 있는 것 같았어요. 사실 그렇지만, 그럼에도 왜 만들었을까요?

우리에게 상자가 주어지면 첫 번째 본능은 `ip`를 nmap하는 것인데, 실제 시나리오에서 '애매모호한 방식으로 보안'도 사용됩니다.

<div class="content-ad"></div>

내가 만든 상자는 실제 아키텍처를 상당히 시뮬레이트하며 펜테스팅이 어떻게 수행되는지를 잘 보여줍니다. 기본 포트에서 실행 중인 SSH는 전 세계의 봇이 IP를 스캔하고 서버를 공격하고 있다는 것을 의미합니다. 키 기반 인증을 사용하더라도 SSH를 표준이 아닌 포트에서 실행하는 것은 로그를 조직하는 좋은 아이디어입니다(실패한 시도가 적을 것입니다).

![image](/assets/img/2024-07-09-TrustIssuesShunyaCTFwrite-up_21.png)

완벽하지는 않았겠지만, 실제 시나리오에서는 상자가 공개되기 전에 테스트될 것입니다 ;) 하지만 "열거가 성공의 열쇠"라는 명언을 밝혀냅니다.

# 이 상자 재현하기

<div class="content-ad"></div>

- 공식 웹 사이트에서 Ubuntu 서버 다운로드: https://releases.ubuntu.com/focal/ (ubuntu 20.04)
- virtualbox 다운로드 (쉽게 .ova 파일을 제공해줍니다. vmware는 잘 모르겠네요): https://www.virtualbox.org/wiki/Downloads
- 우분투 iso를 virtualbox에 가져와서 설정하세요. 너무 많은 RAM이나 SSD가 필요하지 않아요; 2GB, 20GB 정도면 EC2에 배포할 것이므로 충분합니다.
- aalu라는 계정을 만드세요.
- cowrie를 설치하세요. https://cowrie.readthedocs.io/en/latest/INSTALL.html 새로운 사용자를 만들고 백엔드 풀을 생성할 필요가 없습니다. EC2는 그것을 허용하지 않기 때문에 authbind 방법을 사용하세요. iptable 대신 authbind 방법을 사용하는 것이 더 일관성 있습니다.
- 기본 호스트 이름이 srv04인 경우에도 실제 구성 파일에서 호스트 이름을 변경하세요. 사용자 이름을 cowrie에서 aalu로 변경하세요. aalu 사용자를 user.txt 파일에 넣고 root 로그인을 비활성화하세요 (cowrie는 기본적으로 어떤 암호로든 root 로그인을 허용합니다).
- root로 로그인하고 python 바이너리를 womp로 복사하고 UID 비트를 설정하세요.
- /root로 이동하여 100개의 파일을 만드세요 (예: touch flag'1..200') 그리고 그 안에 랜덤한 단어를 넣는 한 줄짜리 스크립트를 만드세요.
- 다시 100개의 파일을 만드세요 (예: touch .flag'150..250') 그리고 반복하세요.
- 플래그를 저장할 파일을 선택하고 플래그를 추가하세요.

# 더 좋게 만들기

상자를 분명히 더 좋고 도전적으로 만들 수 있습니다.

- 특히 TCP/22 및 TCP/420 포트에 대한 포트 제한을 구현하십시오. rustscan 처럼 동작하는 사용자를 1차 시도에서 차단할 수 있도록 ufw를 사용하세요.
- 잘못된 시도가 금지되도록 420번 포트에서 방화벽을 사용하십시오. 이를 위해 fail2ban을 사용하세요. 의도된 방법은 IP 변경기를 사용하는 것이나 난이도를 바꿀 수 있습니다.
https://github.com/fail2ban/fail2ban
- LXD 그룹에서 대상 사용자인 aalu를 제거하세요!!!
- python 이왠지 다른 언어를 사용할 수 있습니다. 저는 rust를 시도했지만 의존성 문제 때문에 실패했습니다. 예를 들어 golang을 시도해볼 수 있습니다.
- privEsc 프로세스를 3번째 사용자를 포함하여 길게 만들 수 있습니다. aalu를 감염한 후, 공격자는 우리가 제공한 privEsc 방법을 통해 3번째 사용자를 감염시켜 root 액세스를 얻기 위해 crontab 취약점을 사용해야 합니다.
- 플래그를 단락으로 넣어 수동으로 플래그를 찾는 것이 어려워지도록 만들어보세요.

<div class="content-ad"></div>

더 할 일이 많지만, 다음 상자의 작성에 포함될 예정이에요.

상자를 즐겼나요? 조언이 필요하시나요? 도와드릴 일이 있으신가요?

디스코드에서 DM을 보내주세요. @tavilefty(또는 tavilefty#7005) 그럼 기꺼이 도와드리겠습니다. 더 많은 컨텐츠를 만들어내려고 저를 팔로우해주세요.