# Redis가 OOM이 발생해버렸다.
Redis는 In-memory 데이터 저장소이다. 떄문에, Memory의 한계점을 기준하여 OOM이 발생할 여지가 있다.

OOM이 발생하게 되면 더이상 메모리를 할당할 수 없어 에러 메시지를 나열한다.
```text
(error) OOM command not allowed when used memory > 'maxmemory'
```

redis에 적재되는 데이터가 점진적으로 증가하는 형상이라면 이러한 메모리 누수에 대한 관리와, 정책을 정의하는 것이 필수이다.

redis에서는 OOM이 발생하지 않도록, maxmemory 설정과 policy를 제공하여, 예상되는 최대 메모리 사용량에 도달했을 때 적용할 전략을 의사결정할 수 있다.

전략에 따라, 메모리에 상주중인 데이터 중 쓸모없는 기준의 우선순위를 의사결정하여, 메모리를 일부 확보하는 방식이다.

- maxmemory-policy
  - volatile-lru : TTL expire set에 설정된 값 중 가장 오랫동안 참조되지 않은 값 삭제
  - allkeys-lru : 가장 오랫동안 참조되지 않은 값을 기반으로 Key 삭제
  - volatile-random : TTL expire set에 설정된 데이터 중 랜덤하게 삭제
  - allkeys-random : 랜덤으로 Key 삭제
  - volatile-ttl : TTL expire set이 설정된 값 중 가장 근 미래에 있는 TTL을 갖는 데이터 부터 삭제
  - noeviction(default) : 캐시를 제거하지 않고, MAX memory에 도달할 경우, 쓰기 오류 반환

> MAX 메모리에 도달했을 때, "야 너 메모리 너가 설정한 최대치에 도달했어. OOM이 발생하면 너무 크리티컬하니깐, 내가 나열해준 전략중에 하나 선택하면
> 내가 생각했을때 좀 쓸모 없어보인다고 생각이 드는 데이터 내가 직접 삭제해줄게."