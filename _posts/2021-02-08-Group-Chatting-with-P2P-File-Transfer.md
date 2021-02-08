---
title : "Group Chatting with P2P File Transfer"
permalink : /projects/group-chatting-with-p2p-file-transfer/

excerpt: "This Project is Group Chatting with file transfer using TCP/IP protocol in Ubuntu Linux Server."

---

안녕하세요, Algorithm 분석을 좋아하는 SKJ입니다. Thinking Projects 두 번째로 소개할 프로젝트는 Socket programming의 일종인 Group Chatting with p2p file transfer입니다. 

학부 시절에 수강했던 임베디드 운영체제 교과목에 팀 프로젝트로 진행했던 과제로, 그룹 채팅과 파일 전송 기능을 수행합니다. 

프로젝트에 관한 소스코드 및 내용은 [SKJ's  Github repository](https://github.com/KeunJuSong/Group-Chatting-with-P2P-File-Transfer)에 있으니 참고하시면 되겠습니다.

# **Idea**

해당 프로젝트를 구현하기 위해선 우선적으로 TCP/IP 프로토콜에 관하여 알고 있어야 합니다. 블로그의 목적 상 TCP/IP에 대한 자세한 설명은 다루지 않겠습니다. 

TCP/IP에 관한 알고리즘을 이해했다면, 그룹 채팅 및 파일 전송 기능을 수행하기 위한 프로세스 선언 및 제어 알고리즘이 필요합니다. 기능을 구현함에 있어 다양한 method가 쓰일 수 있습니다.

저는 그 중 ```pipe()``` 란 method를 사용했으며, 그 이유는 해당 method의 기능이 명확하고 이해하기 쉬웠기 때문입니다. 다른 method를 이용해서도 충분히 구현가능하기 때문에, 다른 방식으로 구현하고 싶으신 분들은 굳이 ```pipe()``` method를 사용하지 않으셔도 됩니다. 

간단하게 ```pipe()```에 관한 기능을 설명하자면, ```pipe()``` 는 부모 프로세스와 자식 프로세스간 통신할 수 있도록 해줍니다.

해당 프로젝트의 목적 중 하나는 그룹 채팅이므로 서버를 통한 클라이언트 간 메시지 통신이 이루어져야 합니다. 

그렇다면, 통신 기능을 지원하는 ```pipe()```와 ```fork()```를 이용해 그룹 채팅을 구현할 수 있겠죠? ```fork()``` 는 프로세스를 복사하여 새로운 프로세스를 만들어내는 method 입니다. ```fork()```에 대한 자세한 내용은 생략합니다.

마지막으로 파일 전송 기능을 수행하기 위해서 우리는 다양한 알고리즘을 설계할 수 있습니다. 저 또한 파일 전송 기능을 수행하기 위해서 어떤 Idea로 구현하면 좋을 지, 다양한 방법으로 trial & error를 시도했습니다. 

여러 방면으로 생각한 결과로, 가장 명확하게 클라이언트 간 파일 전송을 할 수 있는 알고리즘은 다음과 같습니다. 물론 꼭 이 방법이 아니어도 파일 전송 기능을 구현할 수 있습니다. 참고용으로 받아들이시면 좋을 것 같습니다.

* Client1은 파일을 Client2에게 요청한다
* Client2는 파일의 목록 및 내용 message를 Client1에게  전송한다
* Client1은 원하고자 하는 파일의 번호를 입력한다
* Client1에서 수신한 해당 번호의 파일 제목 및 내용을 Client1 내에서 생성한다

 약간의 trick이 있다고 볼 수 있지만, 저는 Client2의 response 기능에 서비스를 확장한 형태라고 생각하기 때문에 trick은 아니라고 생각합니다. 

따라서 제가 생각한 알고리즘의 main point는 Client2의 response 기능으로 Client1에서 자체적으로 Client2 파일과 동일한 파일을 생성할 수 있다는 것입니다.

---

이상으로 Thinking Projects 두번째 프로젝트에 관한 Idea 내용은 여기까지 입니다. 보다 자세한 내용들은 **Googling**을 통해 알아보시길 바랍니다. 또한 앞서 말했듯이 저의 Github repository에도 설명 및 reference가 있으니, 참고하시길 바랍니다. 





