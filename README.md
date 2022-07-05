Binder collaborative Jupyter config issue
=========================================

JupyterLab allows [real time collaboration](https://jupyterlab.readthedocs.io/en/stable/user/rtc.html) between multiple users.

[As shown by @jtpio](https://gist.github.com/jtpio/6ce26381703355e0ef1da4af742b7f72), it is possible to enable the collaborative mode in [Binder](https://mybinder.org/) by putting a `jupyter_config.json` file, with at least this content:

```json
{
    "LabApp": {
        "collaborative": true
    }
}
```

### Bug description

Unfortunately, if the configure is put in a `.binder/` folder, the `jupyter_config.json` file is not taken into account and the collaboration mode is not enabled.

As reported by @adrienpacifico, this file must be located at the root of the project.

### How to reproduce

Issue example (`jupyter_config.json` in `.binder/` folder):

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/seignovert/binder-collab-issue/main?labpath=test.ipynb)

Expected behavior (`jupyter_config.json` at the root of the project):

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/seignovert/binder-collab-issue/expected?labpath=test.ipynb)

Workaround with `postBuild`:
```
if [ -f ~/.jupyter/jupyter_lab_config.py ]
then
    sed -i 's/# c.LabApp.collaborative = False/c.LabApp.collaborative = True/' ~/.jupyter/jupyter_lab_config.py
else
    mkdir -p  ~/.jupyter/
    echo 'c.LabApp.collaborative = True' > ~/.jupyter/jupyter_lab_config.py
fi
```

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/seignovert/binder-collab-issue/postBuild?labpath=test.ipynb)
