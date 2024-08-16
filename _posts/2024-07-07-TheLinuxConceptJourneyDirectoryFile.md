---
title: "리눅스 개념 여정  디렉토리 파일 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-TheLinuxConceptJourneyDirectoryFile_0.png"
date: 2024-07-07 02:49
ogImage: 
  url: /assets/img/2024-07-07-TheLinuxConceptJourneyDirectoryFile_0.png
tag: Tech
originalTitle: "The Linux Concept Journey — Directory File"
link: "https://medium.com/@boutnaru/the-linux-concept-journey-directory-file-74ba52001fc5"
---


Linux에서 사용되는 일곱 가지 다른 파일 유형이 있음을 아시죠 (https://medium.com/@boutnaru/the-linux-concept-journey-linux-file-types-4cb622887331). 이 중 "디렉토리" 파일 유형이 있습니다. 이를 파일과 같이 생각할 수 있으며 해당 콘텐츠로 파일 이름과 해당 i-node 번호를 보유하는 파일입니다 (https://medium.com/@boutnaru/linux-what-is-an-inode-7ba47a519940).

따라서 "rm" 유틸리티 (https://linux.die.net/man/1/rm)를 사용하면 기본적으로 해당 파일을 디렉토리에서 제거하며 삭제하지는 않습니다 ("참조 횟수"가 "0"이 될 때까지). 또한, 파일을 제거하는 것은 파일 자체에 대한 어떠한 권한도 필요하지 않고, 파일을 포함하는 디렉토리에 "쓰기 권한"이 필요합니다.

마지막으로, 새 디렉토리를 생성하려면 "mkdir" 유틸리티 (https://man7.org/linux/man-pages/man1/mkdir.1.html)를 사용하여 새 디렉토리를 만들거나 "rmdir" 유틸리티 (https://man7.org/linux/man-pages/man1/rmdir.1.html)를 사용하여 디렉토리를 제거할 수 있습니다. 아래 스크린샷에서 보여지는 것처럼 디렉토리는 "ls -l" 명령의 출력에서 첫 글자로 "d"로 표시됩니다 (https://man7.org/linux/man-pages/man1/ls.1.html) — 아래의 스크린샷에서도 확인할 수 있습니다.

다음 글에서 뵙겠습니다 ;-) 트위터에서 저를 팔로우하실 수 있습니다 — @boutnaru (https://twitter.com/boutnaru). 또한, 저의 다른 글을 읽을 수 있는 Medium 프로필을 방문하세요 — https://medium.com/@boutnaru. 무료 eBook은 https://TheLearningJourneyEbooks.com에서 찾아보실 수 있습니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-07-TheLinuxConceptJourneyDirectoryFile_0.png)
