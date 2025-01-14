# B 트리, B+ 트리
## B 트리(B - Tree)
B 트리란 데이터베이스와 파일 시스템에서 널리 사용되는 트리 자료구조의 일종으로, 이진 트리를 확장해 **하나의 노드가 가질 수 있는 자식 노드의 개수가 2보다 큰 균형 트리**이다.

**최대 M개의 자식**을 가질 수 있는 B 트리를 **M차 B 트리**라고 한다.

## 특징

1. 노드는 최대 M개의 자식을 가질 수 있다.
   - 3차 B 트리는 최대 3개의 자식 노드를 가질 수 있다.
2. 노드에는 최대 M - 1개의 키를 가질 수 있다.
   - 3차 B 트리는 최대 2개의 키를 가질 수 있다.
3. 각 노드(루트 노드와 리프 노드 제외)는 최소 ⎡M / 2⎤개의 자식 노드를 갖는다.
   - 3차 B 트리에서 각 노드는 최소 2개의 자식을 갖는다.
4. 각 노드(루트 노드 제외)는 최소 ⎡M / 2⎤ - 1개의 키를 갖는다.
   - 3차 B 트리에서 각 노드는 최소 1개의 키를 갖는다.
5. 노드에 키들은 항상 정렬된 상태로 저장된다.

<img width="794" alt="image" src="https://github.com/user-attachments/assets/60cf8864-57ef-4f3a-9420-52c6b5c312a2" />

## 검색
루트 노드에서 대소비교를 시작하여 **하향식으로 검색**을 수행한다.

<img width="399" alt="image" src="https://github.com/user-attachments/assets/71554615-644d-4b55-b28a-7bc90cce769c" />

<br />

<img width="401" alt="image" src="https://github.com/user-attachments/assets/bcd63739-b851-4251-831b-514c8250f238" />

## 삽입
삽입은 항상 리프 노드에서 이루어진다.

삽입하는 과정에서 노드의 키가 M - 1개를 넘으면 가운데 키를 기준으로 좌 / 우 키들을 분할하고 가운데 키는 올라간다.

### Case 1 : 분할이 일어나지 않는 경우
리프 노드가 가득차지 않았다면, **오름차순**으로 삽입한다.

<img width="398" alt="image" src="https://github.com/user-attachments/assets/09439e22-7ca9-44f6-9366-63ef870c160b" />

### Case 2 : 분할이 일어나는 경우
리프 노드에 키가 가득 찬 경우, **노드를 분할**해야 한다.

분할은 **중앙값에서 수행**한다.
중앙값은 **부모 노드로 병합되거나 새로 생성**된다.

왼쪽 키들은 왼쪽 자식으로, 오른쪽 키는 오른쪽 자식으로 **분할**된다.

이후 부모 노드를 검사해서 또 다시 가득 찼다면, **부모 노드에서 다시 분할을 진행**한다.

<img width="393" alt="image" src="https://github.com/user-attachments/assets/f1d1f54c-b480-4899-8702-a08f35f6fe95" />

<br />

<img width="396" alt="image" src="https://github.com/user-attachments/assets/bf392f4a-75a7-4fb2-82cc-00b6472d93cc" />

<br />

<img width="399" alt="image" src="https://github.com/user-attachments/assets/074f3273-5ad3-4aa9-9575-597cf6d87cfc" />

## 삭제
### Case 1 : 삭제할 키 k가 리프 노드에 있는 경우
#### Case 1.1 : 현재 노드의 키 개수가 최소 키 개수보다 큰 경우

1. 다른 노드들에 영향 없이 해당 k를 삭제한다.

<img width="399" alt="image" src="https://github.com/user-attachments/assets/341fdcc8-d975-493c-9403-936020b964c7" />

#### Case 1.2 : 왼쪽 또는 오른쪽 형제 노드의 키가 최소 키 개수 이상인 경우

1. 부모 노드의 키 값으로 k를 대체한다.
2. 최소 키 개수 이상의 키를 가진 형제 노드가 왼쪽 형제 노드라면 가장 큰 값을, 오른쪽 형제 노드라면 가장 작은 값을 부모 노드 키로 대체한다.

<img width="404" alt="image" src="https://github.com/user-attachments/assets/22d823f4-234f-4c47-98ff-914196fe6de0" />

#### Case 1.3 : 왼쪽, 오른쪽 형제 노드의 키가 최소 키 개수이고, 부모 노드의 키가 최소 개수 이상인 경우

1. k를 삭제한 후, 부모 노드의 키를 형제 노드와 병합한다.
2. 부모 노드의 키 개수를 하나 줄이고, 자식 수 역시 하나를 줄여 B 트리를 유지한다.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/e1d1b4e6-66cc-43e1-8c3f-76a61172b6e2" />

#### Case 1.4 : 자신과 형제, 부모 노드의 키 개수가 모드 최소 키 개수인 경우

1. 부모 노드를 루트 노드로 한 부분 트리의 높이가 줄어들기 때문에 재구조화의 경우가 일어난다. 이 경우 Case 3의 2번 과정으로 이동한다.

### Case 2 : 삭제할 키 k가 내부 노드에 있고, 해당 노드나 자식 노드의 키 개수가 최소 키 개수보다 많을 경우

1. 현재 노드의 successor와 k의 자리를 바꾼다.
2. 리프 노드의 k를 삭제하게 되면, Case 1의 경우로 이동한다.

<img width="397" alt="image" src="https://github.com/user-attachments/assets/adb826bf-eb47-4a84-bce1-07091c2d8247" />

### Case 3 : 삭제할 키 k가 내부 노드에 있고, 해당 노드와 자식 노드의 키 개수가 모두 최소 키 개수인 경우

1. k를 삭제하고, k의 양쪽 자식 노드를 병합하여 하나의 노드로 만든다.
2. k의 부모 노드 키를 인접한 형제 노드에 붙힌다. 이후, 이전에 병합했던 노드를 자식 노드로 설정한다.
3. 해당 과정을 수행했을 때 부모 노드의 개수에 따라 이후 수행 과정이 달라진다.

   3 - 1. 만일 새로 구성된 인접 형제 노드의 키가 최대 키 개수를 넘어갔다면, 노드 분할 과정을 수행한다.

<img width="405" alt="image" src="https://github.com/user-attachments/assets/06e1f272-41a8-4b51-8ba5-388d98b1cacd" />

   3 - 2. 만일 인접 형제 노드가 새로 구성되더라도 원래 k의 부모 노드가 최소 키의 개수보다 작아진다면, 부모 노드에 대하여 2번 과정부터 다시 수행해야 한다.

<img width="401" alt="image" src="https://github.com/user-attachments/assets/065675c3-7f96-4623-b2c9-3e0b8c9f10ae" />



