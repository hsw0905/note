# IPC(Inter-Process Communication)

## 프로세스 메모리
- 운영체제 위에서 돌아가는 각 프로세스는 메모리 공간이 독립적으로 주어진다.
- 이 메모리 공간은 외부 다른 프로세스가 접근하는 것을 금지하는데, OS가 이것을 보장한다.

## Shared Memory 기반 IPC
- 프로세스 간에 공유 메모리 설정
- (Process A: 공유 메모리에 쓰기) -> 데이터 변경 -> Signal(윈도우의 경우 Event) 발생 -> (Process B: 데이터 읽기)

## Pipe(File 기반 IPC)
- 파일 기반 단방향 IPC(쓰기만 가능, 혹은 읽기만 가능한 커넥션)
- 양방향 통신의 경우 2개 파이프 생성
- 원격 통신에 부적합

## Socket
- TCP 연결 : Stream Socket
- UDP 연결 : Datagram Socket

## RPC(Remote Procedure Call)
- 원격 프로시저 호출 : 외부 프로세스(원격)와 상호작용 하기 위한 기능
- 네트워크 프로토콜 혹은 IPC 상에서 작동
- 마치 프로세스 자신의 내부 프로시저(메소드)를 호출하는 것 처럼 작동
