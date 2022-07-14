FROM mono:latest
WORKDIR /opt/app

COPY *.csproj .
COPY Program.cs .
RUN csc Program.cs

CMD ["mono", "/opt/app/Program.exe"]