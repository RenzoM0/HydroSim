FROM debian:bullseye-20211201
# Ref https://github.com/thiagoralves/OpenPLC_v3/blob/master/Dockerfile 

ARG script
ARG database

# Install dependencies (inclusief bash, python2 en dos2unix)
RUN apt-get update && \
    apt-get -y install git sqlite3 bash dos2unix python2 && \
    apt-get clean

# Clone en installeer OpenPLC
RUN git clone https://github.com/thiagoralves/OpenPLC_v3.git
WORKDIR /OpenPLC_v3
RUN ./install.sh docker

# Zorg dat er altijd een scripts directory is
RUN mkdir -p /OpenPLC_v3/scripts

# Probeer het meegegeven script te kopiëren, anders fallback gebruiken
# (COPY faalt niet stilletjes, daarom gebruiken we een fallback tijdens runtime)
COPY ${script} /OpenPLC_v3/scripts/script.st
COPY ${database} /OpenPLC_v3/database.sh
COPY entrypoint.sh /OpenPLC_v3/entrypoint.sh

# Line endings fixen + uitvoerbaar maken
RUN dos2unix /OpenPLC_v3/entrypoint.sh && \
    chmod +x /OpenPLC_v3/entrypoint.sh /OpenPLC_v3/database.sh

# Zet een lege fallback script als standaard (gebruikt als compile faalt)
RUN echo "PROGRAM PLC_PRG\nVAR\nEND_VAR\nEND_PROGRAM" > /OpenPLC_v3/scripts/fallback.st

EXPOSE 502
EXPOSE 8080
EXPOSE 20000
EXPOSE 43628

ENTRYPOINT ["/OpenPLC_v3/entrypoint.sh"]
