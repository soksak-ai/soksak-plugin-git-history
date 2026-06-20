# 깃 히스토리 (soksak-plugin-git-history)

프로젝트 커밋 이력 패널. 우측 사이드바에 히스토리(⎇)를 띄우고 커밋을 탐색한다.

- **목록**: 최신순 커밋 50개 — 단축 해시(모노스페이스) + 제목, 작성자·날짜(흐림).
- **페이지네이션**: 하단 "더 보기" 버튼이 다음 50개를 이어 붙인다(skip 누적). "⟳" 새로고침은 목록을 처음부터 다시 받는다.
- **커밋 상세**: 행 클릭 시 목록이 상세로 교체된다 — 메타(제목/해시/작성자/날짜) + 변경 파일 목록(상태 글자 + 경로) + 패치 전문(`<pre>`, 스크롤, 최대 높이 제한). "← 목록"으로 복귀.
- **정직한 실패**: git 저장소가 아닌 폴더·git 오류는 빈 상태에 에러 메시지를 흐린 텍스트로 그대로 보여준다(침묵 실패 없음).
- **안전한 렌더**: 커밋 제목·경로·패치 등 외부 문자열은 전부 `textContent` 로 넣는다 — `innerHTML` 미사용.

## 권한 근거

| 권한 | 근거 |
| --- | --- |
| `ui` | 사이드바에 히스토리 뷰(`history`)를 등록·표시 |
| `git:read` | `git.log`(커밋 목록)·`git.show`(커밋 상세) 읽기 호출 |

그 외 권한은 선언하지 않는다 — 쓰기·파일시스템·명령·네트워크 접근 없음.

## 설치

```bash
# GitHub 단축형
sok plugin.install '{"source":"<user>/soksak-plugin-git-history"}'

# 로컬 경로(이 예제 디렉토리)
sok plugin.install '{"source":"/path/to/repo/examples/plugins/soksak-plugin-git-history"}'

# 활성화(동의는 앱 UI 에서 사람이 직접)
sok plugin.enable '{"id":"soksak-plugin-git-history"}'
```

## 사용법

1. 우측 사이드바 아이콘 레일에서 ⎇(깃 히스토리)를 연다.
2. 커밋 목록을 훑고, 더 오래된 이력은 "더 보기"로 이어 받는다.
3. 커밋을 클릭하면 변경 파일 목록과 패치를 본다. "← 목록"으로 돌아간다.
4. 새 커밋을 만들었으면 "⟳"로 목록을 새로고침한다.

## DOM 노출 (구조적 주소)

호스트가 임의 CSS selector 대신 구조적 path 주소로 DOM 에 접근한다. 이 플러그인이 외부(주소 클릭/측정·E2E)에 노출하는 요소는 매니페스트(`contributes.nodes`)에 선언하고 실제 요소에 `data-node` 속성을 부여한다. 선언/부여 안 된 요소는 접근 불가(`NOT_EXPOSED`).

전역 주소: `win/<label>/<region>/view/soksak-plugin-git-history.history/node/<data-node>`

| 노드 | data-node | 설명 |
| --- | --- | --- |
| `commit` | `commit/<커밋 해시>` | 커밋 행 (클릭하면 상세 보기). 안정키는 전체 커밋 해시(소문자 hex) — 카운터 인덱스가 아님 |
| `more` | `more` | "더 보기" 버튼 (다음 페이지 로드) |
| `refresh` | `refresh` | "⟳" 새로고침 버튼 (목록 처음부터 재로드) |
