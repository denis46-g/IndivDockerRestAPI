# ���������� ����������� ����� .NET SDK ��� ������
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /source

# �������� ����� ������� � ��������������� �����������
COPY *.csproj .
RUN dotnet restore

# �������� ��������� ����� ������� � �������� ����������
COPY . .
RUN dotnet publish -c Release -o /app

# ���������� ����������� ����� .NET Runtime ��� �������
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app .

# �������� ����� ���� ������ � ���������
COPY Database /app/Database

# ��������� ���� 8080 ��� ������� � ����������
EXPOSE 8080

# ������������� sqlite3 ��� ���������� SQL ��������
RUN apt-get update && apt-get install -y sqlite3

# ��������� SQL ������� ��� ������������� ���� ������
RUN sqlite3 /app/Database/superheroes.db < /app/Database/01_reference_data.sql
RUN sqlite3 /app/Database/superheroes.db < /app/Database/02_hero_attribute.sql
RUN sqlite3 /app/Database/superheroes.db < /app/Database/03_hero_power.sql
RUN sqlite3 /app/Database/superheroes.db < /app/Database/04_authentication.sql

# ������ ������� ����������
WORKDIR /app

# ��������� ������� ������� ����������
ENTRYPOINT ["dotnet", "MyRestAPI.dll"]