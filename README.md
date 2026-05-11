# Suspicious_Passages

## Team-Safe Data Directory Setup

Use this pattern so each teammate can keep datasets in a different local directory without creating Git conflicts.

1. Keep shared notebook code in the repo.
2. Store personal paths in a local file (`paths.local.json`) or an environment variable (`DATA_DIR`).
3. Do not commit personal path files.

The repo already ignores personal files in `.gitignore`.

### Local config example

Create a local file named `paths.local.json` in the project root:

```json
{
  "data_dir": "/absolute/path/to/your/data"
}
```

### Notebook code snippet

Add this to your data-loading cell:

```python
from pathlib import Path
import json
import os

project_root = Path.cwd()
default_data_dir = project_root / "data"

env_data_dir = os.getenv("DATA_DIR")
local_cfg_path = project_root / "paths.local.json"

if env_data_dir:
	data_dir = Path(env_data_dir).expanduser()
elif local_cfg_path.exists():
	data_dir = Path(json.loads(local_cfg_path.read_text())["data_dir"]).expanduser()
else:
	data_dir = default_data_dir

csv_path = data_dir / "sample_data.csv"
print("Using data directory:", data_dir)
```

Resolution order is:
1. `DATA_DIR` environment variable
2. `paths.local.json`
3. repo default `data/`
