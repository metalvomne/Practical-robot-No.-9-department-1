import pandas as pd

# 1️ Шлях до твого файлу
file_path = r"C:\Users\metal\Downloads\gdp-per-capita-growth.csv"

# 2️ Зчитуємо CSV у DataFrame
df = pd.read_csv(file_path)

# 3️ Перевіряємо структуру даних
print("Колонки у файлі:")
print(df.columns.tolist())

# 4️ Фільтруємо за 2019 роком
df_2019 = df[df["Year"] == 2019]

# 5️ Зберігаємо дані за 2019 рік
output_2019 = r"C:\Users\metal\Downloads\gdp_per_capita_growth_2019.csv"
df_2019.to_csv(output_2019, index=False, encoding="utf-8")
print(f"\n ✓ Дані за 2019 рік збережено у файлі: {output_2019}")

# 6️ Запитуємо у користувача назви країн
countries_input = input("\nВведіть назви країн через кому (наприклад: Ukraine, Poland, France):\n")
countries = [c.strip() for c in countries_input.split(",") if c.strip()]

# 7️ Шукаємо дані для введених країн (ігноруємо регістр)
results = pd.DataFrame()
for country in countries:
    match = df_2019[df_2019["Entity"].str.lower() == country.lower()]
    if not match.empty:
        results = pd.concat([results, match], ignore_index=True)
    else:
        print(f"!!! Дані для '{country}' не знайдено!")

# 8️ Виводимо результати пошуку
if not results.empty:
    print("\nРезультати пошуку:")
    print(results.to_string(index=False))

    # 9️ Зберігаємо результати у новий файл
    search_output = r"C:\Users\metal\Downloads\search_results.csv"
    results.to_csv(search_output, index=False, encoding="utf-8")
    print(f"\n ✓ Результати пошуку збережено у файл: {search_output}")
else:
    print("\nЗа вказаними країнами нічого не знайдено.")
