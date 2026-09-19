# hellower/lancedb

> [!CAUTION]
> **`hellower/goosedb-lancedb-go`는 LanceDB를 upstream 태그 그대로가 아니라 이 fork(`hellower/lancedb`)의 고정 커밋으로 빌드합니다.**
> `hellower/goosedb-lancedb-go`의 `rust/Cargo.toml`에 있는 `[patch]`가 공식 `v0.39.0` 태그를 `hellower/lancedb@30d3926`으로 바꿔치기합니다.
> 이 커밋은 **`v0.39.0` 태그에 수정 커밋 하나만 얹은 것**이고, 나머지 코드는 upstream 태그와 같습니다.
>
> - **왜 upstream을 그대로 안 쓰나:** upstream 태그(`v0.38.0`, `v0.39.0`)는 `remote` feature 없이
>   빌드하면 **컴파일이 되지 않습니다**. `job.rs`의 `TerminalResult::decode`가 `remote` 전용 variant인
>   `Error::Http`를 조건 없이 사용하기 때문입니다([lancedb/lancedb#4096](https://github.com/lancedb/lancedb/issues/4096)).
>   `hellower/goosedb-lancedb-go`는 `default-features = false`로, `remote` 없이 빌드하므로 매번 이 오류가 납니다.
> - **upstream CI가 못 잡는 이유:** `python/`과 `nodejs/` 바인딩이 `remote`를 기본으로 켜기 때문에,
>   워크스페이스 전체 빌드에서는 feature unification으로 `lancedb`도 `remote`가 켜진 채 컴파일됩니다.
>   `lancedb` 패키지만 `--no-default-features`로 빌드해야 드러납니다.
> - **upstream 수정은 [lancedb/lancedb#4103](https://github.com/lancedb/lancedb/pull/4103)에서 진행 중입니다:**
>   우리가 낸 [#4100](https://github.com/lancedb/lancedb/pull/4100)은 2026-09-03 LanceDB 메인테이너(`wjones127`)의
>   승인을 받았지만, **같은 수정인 #4103과 중복이라 2026-09-19에 우리가 닫았습니다**(#4096도 함께 닫음).
>   **#4103은 2026-09-19 현재 열려 있고 아직 승인 전(REVIEW_REQUIRED)입니다.**
> - **upstream `main`과 `v0.40.0-beta.3`도 아직 고쳐지지 않았습니다.** `v0.40.0`으로 올릴 때도 fork
>   브랜치를 다시 만들어야 할 가능성이 높습니다.
> - **`[patch]` 제거 조건:** #4103이 머지되고 그 뒤의 LanceDB 태그가 나와서, 그 태그가 `[patch]` 없이
>   컴파일되면 지웁니다.
>
> **수정은 이 fork의 `main`이 아니라 태그별 브랜치에 있습니다.**
>
> | LanceDB 태그 | 브랜치 | 커밋 | 부모(태그 커밋) |
> | --- | --- | --- | --- |
> | `v0.39.0` (현재 사용) | [`fix/v0.39.0-job-remote-gate`](https://github.com/hellower/lancedb/tree/fix/v0.39.0-job-remote-gate) | `30d3926` | `0c33b27` |
> | `v0.38.0` | [`fix/v0.38.0-job-remote-gate`](https://github.com/hellower/lancedb/tree/fix/v0.38.0-job-remote-gate) | `9437133` | `8c68e0c` |
>
> 기술적 세부 내용과 LanceDB를 올릴 때 패치를 다시 만드는 절차는 `hellower/goosedb-lancedb-go`(비공개)
> README의 "upstream 대비 커스텀 패치" 섹션에 있습니다.
