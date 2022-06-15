# React Query

React Query는 서버 상태를 관리하기 위한 라이브러리로 클라이언트와 서버의 비동기 로직을 손쉽게 다룰 수 있도록 돕는다.

React Query에서 말하는 서버 상태란 다음과 같다.

- 클라이언트에서 제어하거나 소유하지 않는 위치에서 관리 및 유지된다.
- Fetching 및 Updating를 위한 비동기 API가 필요하다.
- 다른 사람들과 공유되는 것으로 사용자가 모르는 사이에 변경될 수 있다.
- 신경 쓰지 않는다면 잠재적으로 오래된 상태가 될 가능성이 있다.

즉, 입력중인 값의 상태나 모달이 열려있는지에 대한 상태는 클라이언트 상태이며 API요청을 통해 가져오는 데이터를 서버 상태라고 정의할 수 있다.

위와 같은 서버 상태를 효율적으로 관리하기 위해 React Query는 여러 기능을 제공한다.

- 동일한 데이터에 대한 여러 요청을 단일 요청으로 중복 제거
- 데이터 캐싱 등..

그럼 React Query에 대해서 살펴보자.

## React Query의 상태

1. fresh : 새롭게 추가된 쿼리로써, 만료되지 않은 쿼리다 ⇒ 컴포넌트 마운트, 업데이트시에 데이터를 재요청 하지 않는다.
2. fetching : 요청중인 쿼리
3. stale : 만료된 쿼리 ⇒ 컴포넌트 마운트, 업데이트시에 재요청한다.
4. inactive : 비활성화된 쿼리 ⇒ 5분 뒤에 가비지 컬렉터가 캐시에서 제거한다.

<br>

## React Query의 re-fetching

1. stale 상태인 쿼리 인스턴스가 새로 만들어졌을 때(mount되었을 때)
2. 윈도우가 다시 포커싱 되었을 때
3. 네트워크가 다시 연결되었을 때
4. refetchInterval 옵션이 있을 때

✅ 데이터를 수정하거나, 삭제해서 re-fetching 하고 싶을 경우 queryClient의 invalidateQueries 메서드로 해당 쿼리 키를 무효화하여 re-fetching 할 수 있다.

<br>

## React Query의 caching

리액트 쿼리를 사용한다고 해서 데이터가 자동으로 캐싱되는것은 아니다.

캐싱을 위해서는 staleTime과 cacheTime 에 대해 이해가 필요하다.

### staleTime (기본값 : 0)

- 데이터가 fresh ⇒ stale 상태로 변경되는데 걸리는 시간
- 데이터가 fresh인 상태에서는 쿼리 인스턴스가 새로 만들어져도(mount 되어도) re-fetching이 발생하지 않는다.
- 즉, 첫번째 fetching 이후 staleTime이 지나지 않았다면 unmount후 mount 되어도 re-fetching이 발생하지 않는다.
- staleTime의 기본값은 0이기 때문에

### cacheTime (기본값 : 5분)

- 데이터가 inactive된 시점을 기준으로 캐싱된 상태로 남아있는 시간으로써, 쿼리 인스턴스가 unmount되면 데이터는 inactive 상태로 변경된다.
- cacheTime이 지나기 전에 쿼리 인스턴스가 다시 마운트되면 데이터를 fetch하는 동안 캐시 데이터를 보여준다.

✅ refetch 메서드는 캐싱 결과를 조회하지 않고 요청을 날린다는 점을 주의하자

## 참고 자료

https://react-query.tanstack.com/overview <br>
https://darrengwon.tistory.com/1517 <br>
https://techblog.woowahan.com/6339/
