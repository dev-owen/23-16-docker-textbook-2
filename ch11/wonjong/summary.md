---
marp: true
---

# 11장. 도커와 도커 컴포즈를 이용한 애플리케이션 빌드 및 테스트

---

## 11.1 도커를 이용한 지속적 통합 절차

CI 절차
- 최신 상태의 코드를 받아오기
- 도커 이미지 빌드 (빌드 단계)
- 컨테이너 실행 (테스트 단계)
- 레지스트리에 이미지 푸시 (배포 단계)

실제 과정은 컨테이너 내부에서 진행
- 컴파일러나 SDK를 서버에 직접 설치할 필요가 없음

---

## 11.2 도커를 이용한 빌드 인프라스트럭처 구축하기

실습
- 형상 관리 기능 (Gogs)
- 이미지 배포 (도커 레지스트리)
- 자동화 서버 (Jenkins)

---

ch11/exercises/infrastructure/docker-compose-linux.yml

~~~
version: "3.7"

services:
  jenkins:
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
~~~

---

ch11/exercises/infrastructure/docker-compose.yml

~~~
version: "3.7"

services:
  gogs:
    image: diamol/gogs
    ports:
      - "3000:3000"
    networks:
      - infrastructure

  registry.local:
    image: diamol/registry
    ports:
      - "5000:5000"
    networks:
      - infrastructure

  jenkins:
    image: diamol/jenkins
    ports:
      - "8080:8080"
    networks:
      - infrastructure

networks:
  infrastructure:
    name: build-infrastructure
~~~

