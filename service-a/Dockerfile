# Service A Dockerfile
FROM python:3.11

WORKDIR /app

COPY . .

# Install curl + gunicorn + app dependencies
RUN apt-get update && apt-get install -y curl \
    && pip install --no-cache-dir -r requirements.txt

ENV SECRET_KEY=supersecret123
ENV DB_PASSWORD=admin1234

EXPOSE 5000

USER root

#CMD ["python", "app.py"]

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
