FROM ubuntu:focal

RUN apt update && apt install python3 && apt autoclean

EXPOSE 8000

CMD ["python3 -m http.server"]
