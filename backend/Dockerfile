FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Destroy any .env file that accidentally got copied from your Windows machine
RUN rm -f .env

# Hardcode the variables into the image environment
ENV SECRET_KEY="super-secret-key"
ENV SQLALCHEMY_DATABASE_URI="postgresql://admin:adminpass@cumin-db:5432/cumin_dashboard"

CMD ["gunicorn", "-b", "0.0.0.0:5000", "app:app"]