FROM python:3.11-alpine
WORKDIR /app
COPY app.py .

RUN apk add --no-cache curl
# Непривилегированный пользователь + permissions
RUN adduser -D appuser && chown -R appuser /app 
USER appuser

EXPOSE 8080
CMD ["python", "app.py"]