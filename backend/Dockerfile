# Stage 1: Tests et qualité
FROM python:3.12 as test
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt flake8 bandit

COPY . .
# Tests de qualité
RUN flake8 app/ --count --show-source --statistics || true
RUN bandit -r app/ -f json -o /tmp/bandit-report.json || true

# Stage 2: Application runtime
FROM python:3.12-slim as runtime
WORKDIR /app

# Non-root user pour la sécurité
RUN useradd -m -u 1000 appuser

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY --from=test /app .
RUN chown -R appuser:appuser /app

USER appuser
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
