# Setip_Rustdesk_Server_On_Portainer

docker(portainer)가 설치되있다는 점과 공유기 포트포워딩이 사진처럼 되어있다는 전제 하에 설명 
(Lan IP는 본인 라즈베리파이ip혹은 rustdesk 서버로 쓸 ip (본인은 사전에 DHCP설정해둬서 ex)192.168.0.125와 같이 세팅)

<img width="548" height="143" alt="2026-03-26 01 02 45" src="https://github.com/user-attachments/assets/38acf735-e763-47b5-8ff9-53c7646c69f2" />


1. 왼쪽 메뉴에서 Stacks 클릭 후 Add Stack 클릭

2. Name을 rustdesk-server로 지정 후 다음 코드 작성 

version: '3'

services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    command: hbbs -r [공인ip혹은도메인(대괄호 지우기)]:21117 -k _
    volumes:
      - ~/rustdesk/data:/root
    ports:
      - 21115:21115
      - 21116:21116
      - 21116:21116/udp
      - 21118:21118
    restart: unless-stopped
    networks:
      - rustdesk-net

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest
    command: hbbr -k _
    volumes:
      - ~/rustdesk/data:/root
    ports:
      - 21117:21117
      - 21119:21119
    restart: unless-stopped
    networks:
      - rustdesk-net

networks:
  rustdesk-net:
    external: false

3. 다음 코드 작성 후 Deploy the stack 클릭
4. hbbr, volumes 부분의 (최상위디렉토리)/rustdesk/data부분의 경로로 가보면 id_ed25519.pub 라는 파일확인 (그게 API key)
