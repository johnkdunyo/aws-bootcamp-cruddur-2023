# Week 1 — App Containerization

- [Get the apps running locally](#get-the-apps-running-locally)
- [Containerize backend and frontend](#containerize-backend-and-frontend)
- [Add notification endpoint and React page](#add-notification-endpoint-and-react-page)
- [Compose multiple containers with DynamoDB Local and Postgres](#compose-multiple-containers-with-dynamodb-local-and-postgres)
- [Homework challenges](#homework-challenges)
- [References](#references)

## Get the apps running locally

### 1 . Running the Flask app locally

For the backend, at the root of the project, i run the following commands to run the flask app locally on port 4567.
I created a virtual env to for the backend app

```sh
cd backend-flask
python3 -m venv .venv
source .venv/bin/activate
pip3 install -r requirements.txt
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
```

I got the following results in my terminal
![Proof of flask json](assets/week1-screenshot-running-flask-app-terminal.png)

And i went ahead to confirm the api from my browser using the `http://localhost:4567/api/activities/home`

![Proof of flask json](assets/week1-screenshot-running-flask-app.png)

### 2 . Running the React app locally

For the frontend, at the root of the project, i run the following commands

```sh
cd frontend-react-js
npm i
```

## Containerize backend and frontend

I created a Dockerfile for the flask app; `backend-flask/Dockerfile` ([code](https://github.com/beiciliang/aws-bootcamp-cruddur-2023/blob/week-1/backend-flask/Dockerfile)). At the root, I run the commands below to build a docker image and run it.

```sh
docker build -t  backend-flask ./backend-flask
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
```

We can send curl to test the server, which returns json as the response in the previous section:

```sh
curl -X GET http://localhost:4567/api/activities/home -H "Accept: application/json" -H "Content-Type: application/json"
```
