# Service A Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY . .

# Install curl + gunicorn + app dependencies
RUN apt-get update && apt-get install -y curl \
    && pip install --no-cache-dir -r requirements.txt \
    && useradd -m appuser

USER appuser

EXPOSE 5000

#CMD ["python", "app.py"]

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
