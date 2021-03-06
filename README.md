# ibm-watson-python-web
MassChallenge 2017 (Oct 6 - Oct 8) 
Tutorial led by Chyld on Oct 7

How to Create a Simple Running Flask App with IBM Watson Features

Prerequesites: 
- Python vers. 2.7 or 3.6
- `pip install Flask`
- `pip install IPython` (for debugging purposes)

1. Create a simple `main.py` Flask file 
```
import sys
import json
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/', methods=['GET'])
def home():
  return "<h1>hello world</h1>"

@app.route('/version', methods=['GET'])
def version():
  return sys.version

# last line of code
app.run(host='0.0.0.0', port=3333, debug=True)
```

2. Log into IBM Bluemix at https://console.bluemix.net/

3. Download correct CloudFoundry (cf) CLI package at https://github.com/cloudfoundry/cli/releases

4. Create a `manifest.yml` file

```
applications:
- disk_quota: 1024M
  host: <YOUR-NAME>-python
  name: <YOUR-NAME>-python
  path: .
  domain: mybluemix.net
  instances: 1
  memory: 256M
  buildpack: https://github.com/cloudfoundry/buildpack-python.git#v1.5.18
  command: python main.py
```

5. Create a `requirements.txt` file

```
Flask==0.12
watson-developer-cloud==0.25.2
```

6. Create a `runtime.txt` file
`python-3.6.1`

7. In the terminal, `> cf push`

8. Go back to `main.py` and insert IBM Watson Tone Analyzer as a POST request

```
@app.route('/tone', methods=["POST"]) #POST of tone
def post_tone():
	u = <YOUR-USERNAME>
	p = <YOUR-PASSWORD>
	v = "2016-05-19"
	text = request.values['tone'] #from dictionary, textarea name = tone, pull out input words
	t = ToneAnalyzerV3(username=u, password=p, version=v)
	d = t.tone(text)
	
	from IPython import embed; embed() #COOL DEBUGGING TECHNIQUE
	return render_template("tone.html")
  ```
  
  9. Run `> python main.py` and in debugger mode, type `> d` to view results of Tone Analyzer
  
  
  Lesson summarized by Celine Yan
