# Claviger SDKs

Python and TypeScript clients that download verified model packages to a local directory.
Packages are MIT-licensed and install from public GitHub Releases without a GitHub login.

## Install

Python 3.10+:

```bash
python -m pip install https://github.com/speridlabs/claviger-sdk/releases/download/claviger-sdk-v0.3.1/claviger_sdk-0.3.1-py3-none-any.whl
```

Node.js 22+ with ES modules:

```bash
npm install https://github.com/speridlabs/claviger-sdk/releases/download/claviger-sdk-v0.3.1/claviger-sdk-typescript-0.3.1.tgz
```

Pin the version and retain dependency lockfiles. Release assets include `SHA256SUMS`.

## Configure and resolve

SDK 0.3.0 and later connect to `https://claviger.speridlabs.com` by default; set
`CLAVIGER_ENDPOINT` only for another deployment. Release 0.2.0 has no built-in endpoint and requires
`CLAVIGER_ENDPOINT=https://claviger.speridlabs.com`. Obtain model access from your administrator
and supply a token through `CLAVIGER_TOKEN` when required; keep credentials out of source code.

Python:

```python
from claviger_sdk import Claviger

client = Claviger()
model_dir = client.resolve("speridlabs-model-registry://gpt2")
```

TypeScript:

```typescript
import { Claviger } from "@speridlabs/claviger";

const client = new Claviger();
const modelDir = await client.resolve("speridlabs-model-registry://gpt2");
```

Replace `gpt2` with a published model from your catalog. Both SDKs return a directory whose files
have been verified against the model manifest. Install PyTorch, Transformers or other runtimes
separately. Full SDK references are included in the packages and source release archive.
