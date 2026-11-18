# Day 8 - Python II: web apps with Flask

## Session goal

Run the course's Flask URL-shortening app in the Ubuntu lab, understand how
routes connect URLs to Python functions, and add one small feature such as a
health route or visit counter. Keep the app for the Day 12 monitoring and Day
14 container exercises.

Suggested time: 90-120 minutes.

## 1. Fundamentals

Flask is a Python web framework. A route maps a path and HTTP method to a
function. The function returns a response. For example, `GET /health` can
return a small health response; `POST /shorten` can accept input and return a
short URL. A route is application behavior, while an HTTP status code describes
the result of a request.

The development server is for local learning. Debug mode exposes an interactive
debugger and must not be enabled on a public or production server. Bind the lab
to `127.0.0.1` so it is reachable only from the same machine.

## 2. Practical lab

### 2.1 Use an isolated Python environment

```bash
mkdir -p ~/sre-lab/day8
cd ~/sre-lab/day8
python3 -m venv .venv
source .venv/bin/activate
python -m pip install Flask
```

Follow the course's URL-shortener project instructions to create or copy its
app files into this folder. Keep a `requirements.txt` for dependencies:

```bash
python -m pip freeze > requirements.txt
```

### 2.2 Run and inspect the app

Replace `app` below with the module name used by your course project:

```bash
flask --app app run --debug --host 127.0.0.1
```

Open the app at `http://127.0.0.1:5000` in the lab browser. In another
terminal, send a request:

```bash
curl -i http://127.0.0.1:5000/
```

Press `Ctrl+C` in the server terminal to stop the development server.

### 2.3 Add a small route

If the course app is a normal Flask module with an `app` object, add a route
like this and adjust its name to match the project:

```python
@app.get("/health")
def health():
    return {"status": "ok"}, 200
```

Restart the app and verify:

```bash
curl -i http://127.0.0.1:5000/health
```

Expected: a 200 response and a small JSON body. If the course app uses an
application factory or different structure, adapt the route to that pattern.

## 3. Review and Day 8 notes

1. What is a route?
2. Which HTTP method does a browser usually use to load a page?
3. What does the health route return?
4. Why is Flask's built-in server intended for development?

- App start command:
- Route added and response:
- Three things learned:
  1.
  2.
  3.
- Confusing command / resolution:
- Idea for what could break at 100 users:

## Done when

- [ ] The Flask app runs in the lab and opens in a browser.
- [ ] The new route or visit counter works.
- [ ] I can explain what an HTTP route does.
- [ ] I have kept the app files for later sessions.

## Further reading

- [Flask installation](https://flask.palletsprojects.com/en/stable/installation/)
- [Flask quickstart](https://flask.palletsprojects.com/en/stable/quickstart/)

