# FlatCAM-on-MacOS
How to make FlatCAM (V8.994) work on MacOS (12.7.6)

# Download the sources from:
https://bitbucket.org/jpcgt/flatcam/downloads/

# originally from:
https://github.com/larroy/flatCAM?tab=readme-ov-file


# First install python3.8  (higher than 3.11 doesn’t work)
brew install python@3.8

# Then install PyQt-5  (other versions don’t work)
brew install PyQt@5

# Then install all other FlatCAM dependencies with
python3.8 -m pip install -r requirements.txt

# Then change the versions of vispy and shapely
pip3.8 uninstall vispy
pip3.8 install vispy==0.7

pip3.8 uninstall shapely
pip3.8 install shapely==1.7.0


# Then change some code … :

# For Error: “No module named 'ezdxf.math.vector’ ” in v8.994 BETA

In “appParsers/ParseDXF.py”

OLD:
from ezdxf.math.vector import Vector as ezdxf_vector


Change to NEW:
try:
    from ezdxf.math.vector import Vector as ezdxf_vector
except ImportError:
    from ezdxf.math import Vec3 as ezdxf_vector



# For Error no “Inf in numpy”

In files  “appTools/ToolPaint”  and  “appTools/ToolCutOut” and “appTools/ToolDblSided”  and  “appObjects/ObjectCollection“

OLD:
from numpy import Inf


Change to NEW:
from numpy import inf as Inf


# Then run it with
python3.8 FlatCAM.py

# Or better even - create an Automator script (Run Shell Script):
“Shell: /bin/bash”       “Pass input: to stdin”

/usr/local/bin/python3.8 /Users/XXX/PCB-Isolation-Milling/FlatCAM/FlatCAM.py
