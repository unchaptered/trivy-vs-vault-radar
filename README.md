# Trivy vs Vault Radar

본 문서는 Trivy와 Vault Radar의 성능, 속도 등을 비교하고 있습니다.

실습에 사용한 스크립트는 [Trivy Use Case (ko)](https://github.com/unchaptered/trivy-use-case/blob/main/docs/README_ko.md)와 [Vault Radar Use Case (ko)](https://github.com/unchaptered/vault-radar-use-case/blob/main/docs/README_ko.md)를 참고하였습니다.

> 저장소 목적상 결과를 서두에 배치하고 재연을 위한 [설치](./READMD_ko.md#설치), [설정](./READMD_ko.md#설정) 및 [시작](./READMD_ko.md#시작) 순으로 배치하였습니다.

## 번역본

- Korean : [open](./docs/READMD_ko.md)
- English : [open](./docs/READMD_en.md)

## 결과

(특정한 설정을 하지 않고) <br>
Trivy에 비해서 Vault Radar가 전체적인 탐지 성능이 뛰어난 것을 볼 수 있습니다. <br>
특히 특정한 문자열 패턴 (API Key*, DB Params*) 군을 인식하는 부분이 좋았습니다.

둘 모두 아쉬운 점으로는 각종 암호화된 문자열에 대해서는 탐지가 되지 않는다는 점이었습니다. <br>
LOG, MIDDLE 레벨로도 디텍트가 되고 이를 리뷰할 수 있으면 좋곘다는 생각을 했습니다.

<details>
<summary>Trivy 결과</summary>

<img src="result-trivy.png" style="width:800px;"/>
</details>

<details>
<summary>Vault Radar 결과</summary>

<img src="result-vault-radar.png" style="width:800px;"/>
</details>

| No. | Option                                               | Trivy                | Vault Radar                           |
| --- | ---------------------------------------------------- | -------------------- | ------------------------------------- |
| 1   | API Key (aws, gcp, ncp, azure)                       | 제한된 형태만 감지\* | 대다수의 형태 감지\*                  |
| 2   | X.509 (key, csr, crt key)                            | `key`만 감지         | `key` 만 감지                         |
| 3   | RSA, ED25519 (priv, pub key)                         | `priv`만 감지        | `priv` 만 감지                        |
| 4   | Encoded String (base32, base64)                      |                      |                                       |
| 5   | MD5, SHA-1, SHA-256, SHA3-256, PBKDF2, Argon2        |                      |                                       |
| 6   | Random String                                        |                      |                                       |
| 7   | DB Params (MongoDB URI, RDS URI, Username, Password) |                      | 특정한 형태의 키만 감지(예, password) |
| 8   | AI Model                                             |                      |                                       |
| 9   | Blockchain Contract                                  |                      |                                       |
| 10  | PCI-SSC (Username, Location, ID, Passport)           |                      |                                       |

## 설치

```shell
brew install gh
brew install trivy
brew install vault-radar
```

## 설정

Vault Radar 실습을 위해서 이 [문서](https://github.com/unchaptered/vault-radar-use-case/blob/main/docs/README_ko.md#%EC%84%A4%EC%A0%95)를 참고해주세요.

```shell
DOCKER_API_VERSION=1.45
VAULT_RADAR_GIT_TOKEN="<PAT>"   # GitHub Personal Access Token
HCP_PROJECT_ID="<VALUE>"        # HCP Vault Radar 생성하면 할당받아짐
HCP_CLIENT_ID="<VALUE>"         # HCP IAM 생성 후 별도로 Key 발급해야함
HCP_CLIENT_SECRET="<VALUE>"     # HCP IAM 생성 후 별도로 Key 발급해야함
```

## 시작

설정이 완료되면 아래 명령어를 통해서

```shell
trivy fs . -f sarif > trivy.sarif

DOCKER_API_VERSION=1.45           \
    VAULT_RADAR_GIT_TOKEN=<VALUE> \
    HCP_PROJECT_ID=<VALUE>        \
    HCP_CLIENT_ID=<VALUE>         \
    HCP_CLIENT_SECRET=<VALUE>     \
    vault-radar scan folder       \
        -p ./                     \
        -f sarif                  \
        -o sarif.log
```
