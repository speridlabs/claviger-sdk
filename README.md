# Claviger SDKs

Python and TypeScript clients for authenticated, verified model downloads. This repository distributes the SDKs; development takes place upstream. Each release includes installable packages, the corresponding source code, and SHA-256 checksums. Only the SDKs are covered by the MIT license in this repository.

## Install without GitHub authentication

The current release is `claviger-sdk-v0.1.1`. Download packages and `SHA256SUMS` from the [release page](https://github.com/speridlabs/claviger-sdk/releases/tag/claviger-sdk-v0.1.1), verify the matching checksum, and install the local file:

```sh
python -m pip install ./claviger_sdk-0.1.1-py3-none-any.whl
npm install ./claviger-sdk-typescript-0.1.1.tgz
```

Python requires 3.10 or newer; the TypeScript client targets Node.js 22 or newer. Installing the packages does not require a GitHub account, npm credentials, or a PyPI publication. Standard package dependencies are downloaded from their respective registries.

For automation, pin the exact release and an independently approved SHA-256 digest. A checksum downloaded alongside a package checks consistency; an independently pinned digest also anchors the expected artifact. Never embed a service credential in an installation URL or lockfile.

## Configure access separately

The SDKs do not contain a deployment address. Obtain your endpoint and access instructions from your administrator. Set `CLAVIGER_ENDPOINT` to that HTTPS endpoint, or configure it in a private SDK profile. Keep endpoints and credentials out of public source code and release assets.

Model access still requires one of the authentication methods configured by your deployment: Tailscale identity, a Claviger bearer token, or AWS credentials with SigV4. Public installation does not grant access to any model. A CI job can acquire AWS credentials through its own OIDC role; a GitHub download credential is unnecessary.

```python
from claviger_sdk import Claviger
from transformers import AutoModelForCausalLM, AutoTokenizer

claviger = Claviger()
model_dir = claviger.resolve("speridlabs-model-registry://example/gpt2")
tokenizer = AutoTokenizer.from_pretrained(model_dir)
model = AutoModelForCausalLM.from_pretrained(model_dir)
```

Install Transformers and your preferred model runtime separately. `resolve()` returns a verified local directory suitable for consumers that accept local model paths.

```typescript
import { Claviger } from "@speridlabs/claviger";

const claviger = new Claviger();
const modelDir = await claviger.resolve("platform-models://example/model");
```

The model references above are synthetic examples. The available namespaces, models, and permissions depend on your deployment. Both SDKs support private profiles and a verified local cache; consult their READMEs in the source archive for configuration and offline use.

## Source and reproducible development inputs

Download `claviger-sdk-source-0.1.1.tar.gz` from the same release. It contains SDK source, tests, dependency lockfiles, build configuration, individual SDK documentation, and the shared synthetic manifest fixture. It contains no backend, infrastructure, deployment configuration, or private Git history.

After extracting the archive, run these commands from its root. Install uv, Node.js, and the pnpm version declared in `sdks/typescript/package.json` first.

```sh
uv run --project sdks/python --frozen --extra test pytest sdks/python/tests -q
uv build --project sdks/python --wheel --out-dir dist
pnpm --dir sdks/typescript install --frozen-lockfile
pnpm --dir sdks/typescript typecheck
pnpm --dir sdks/typescript test
pnpm --dir sdks/typescript pack --out ../../dist/claviger-sdk-typescript-0.1.1.tgz
```

The source and dependency inputs are included so recipients can inspect, modify, and rebuild the clients. Byte-for-byte reproducibility of all toolchain outputs is not guaranteed. GitHub's automatically generated repository source archives contain the distribution repository; use the explicitly attached `claviger-sdk-source-0.1.1.tar.gz` for the SDK implementation.

Report reproducible SDK issues through this repository's issue tracker. Remove deployment addresses, model metadata, tokens, signed download URLs, and personal information from public reports.
