FROM python:3.8-slim

# update package lists from default debian repositories
RUN apt-get update \
    # install necessary packages
    && apt-get install -y curl gnupg lsb-release \
    # fetch the signing key of postgresql repository, convert it into gpg format and save it
    && curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /usr/share/keyrings/postgresql-keyring.gpg \
    # add postgresql repository
    && echo "deb [signed-by=/usr/share/keyrings/postgresql-keyring.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/postgresql.list \
    # update package lists again to include PostgreSQL repository packages
    && apt-get update \
    # install postgresql-client in the latest version
    && apt-get install -y postgresql-client

COPY start.sh /app/start.sh

COPY elt_script.py .

WORKDIR /app

RUN "echo 0 3 * * * python /app/elt_script.py" | crontab -

CMD ["python", "elt_script.py"]