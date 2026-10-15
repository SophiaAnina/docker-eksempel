FROM python:3.12-alpine AS  builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.12-alpine
WORKDIR /app
ENV FLASK_APP=app.py \
    FLASK_RUN_HOST=0.0.0.0 \
    FLASK_RUN_PORT=80     
COPY --from=builder /install /usr/local
RUN adduser -D appuser
COPY --chown=appuser:appuser . .
EXPOSE 5000
CMD [ "flask","run" ]



