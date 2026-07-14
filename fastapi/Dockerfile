FROM python:3.13-slim
WORKDIR /fastapi-app
COPY . .
RUN pip install fastapi[standard]
EXPOSE 8000
CMD [ "fastapi", "run", "main.py", "--host", "0.0.0.0", "--port", "8000" ]