# Используем официальный образ .NET SDK для сборки
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /source

# Копируем файлы проекта и восстанавливаем зависимости
COPY *.csproj .
RUN dotnet restore

# Копируем остальные файлы проекта и собираем приложение
COPY . .
RUN dotnet publish -c Release -o /app

# Используем официальный образ .NET Runtime для запуска
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app .

# Копируем файлы базы данных в контейнер
COPY Database /app/Database

# Открываем порт 8080 для доступа к приложению
EXPOSE 8080

# Устанавливаем sqlite3 для выполнения SQL скриптов
RUN apt-get update && apt-get install -y sqlite3

# Выполняем SQL скрипты для инициализации базы данных
RUN sqlite3 /app/Database/superheroes.db < /app/Database/01_reference_data.sql
RUN sqlite3 /app/Database/superheroes.db < /app/Database/02_hero_attribute.sql
RUN sqlite3 /app/Database/superheroes.db < /app/Database/03_hero_power.sql
RUN sqlite3 /app/Database/superheroes.db < /app/Database/04_authentication.sql

# Задаем рабочую директорию
WORKDIR /app

# Указываем команду запуска приложения
ENTRYPOINT ["dotnet", "MyRestAPI.dll"]