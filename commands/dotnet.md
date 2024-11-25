### Общие команды .NET

1. **Создание проекта**
   - **Web API:**
     ```bash
     dotnet new webapi -n MyApiProject
     ```
   - **Class Library:**
     ```bash
     dotnet new classlib -n MyClassLibrary
     ```
   - **Console App:**
     ```bash
     dotnet new console -n MyConsoleApp
     ```

2. **Создание solution:**
   ```bash
   dotnet new sln -n MySolution
   ```

3. **Добавление проекта в solution:**
   ```bash
   dotnet sln add <путь_к_проекту>
   ```

4. **Добавление ссылки на другой проект:**
   ```bash
   dotnet add <путь_к_проекту_назначения> reference <путь_к_проекту_источнику>
   ```

5. **Восстановление зависимостей проекта:**
   ```bash
   dotnet restore
   ```

6. **Сборка проекта:**
   ```bash
   dotnet build
   ```

7. **Запуск проекта:**
   ```bash
   dotnet run
   ```

8. **Запуск тестов:**
   ```bash
   dotnet test
   ```

9. **Публикация приложения:**
   Чтобы опубликовать приложение, выполните:
   ```bash
   dotnet publish -c Release -o ./publish
   ```
   Параметр `-c Release` указывает конфигурацию сборки, а `-o` — папку, куда будет размещено опубликованное приложение.

### Управление пакетами (NuGet)

1. **Добавление NuGet пакета:**
   Чтобы добавить пакет, используйте команду:
   ```bash
   dotnet add <проект> package <имя_пакета>
   ```

2. **Просмотр установленных пакетов:**
   Чтобы увидеть все установленные NuGet пакеты в проекте:
   ```bash
   dotnet list package
   ```

3. **Удаление NuGet пакета:**
   ```bash
   dotnet remove <проект> package <имя_пакета>
   ```

### Работа с миграциями (для Entity Framework Core)

1. **Добавление миграции:**
   ```bash
   dotnet ef migrations add <имя_миграции>
   ```

2. **Применение миграций:**
   ```bash
   dotnet ef database update
   ```

3. **Удаление последней миграции:**
   ```bash
   dotnet ef migrations remove
   ```

4. **Сброс базы данных (удаление и повторное создание):**
   ```bash
   dotnet ef database drop
   ```

### Дополнительные команды

1. **Очистка проекта (удаление собранных файлов):**
   ```bash
   dotnet clean
   ```

2. **Информация о версии SDK:**
   Чтобы увидеть установленную версию .NET SDK:
   ```bash
   dotnet --version
   ```

3. **Общие сведения о .NET SDK и проекте:**
   ```bash
   dotnet --info
   ```

