# Інструкція по встановленню Elixir v3 Validator Node

## 1. Створення нових EVM гаманців

1. Створіть два нових EVM гаманці:
   - Перший гаманець підписуємо "Elixir v3".
   - Перейдіть на [Sepolia Faucet](https://sepoliafaucet.com) та отримайте тестові ETH в мережі Sepolia. Достатньо 0.1 ETH.
   - Якщо у вас є тестові токени Sepolia на іншому гаманці, перекиньте їх на "Elixir v3".
   - Створіть другий гаманець, назвіть його "elixir-empty". Він повинен бути порожнім і без жодної транзакції. Вам потрібен лише його private key.

2. Не забудьте зберегти seed phrase та private key для обох гаманців.

## 2. Підготовка до встановлення та запуску ноди

1. Оновіть систему:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Встановіть необхідні пакети:
   ```bash
   sudo apt install make clang pkg-config libssl-dev build-essential git gcc chrony curl jq ncdu bsdmainutils htop net-tools lsof fail2ban wget -y
   ```

3. Завантажте скрипт для встановлення Docker:
   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   ```

4. Встановіть Docker:
   ```bash
   sudo sh get-docker.sh
   ```

5. Перевірте версію Docker:
   ```bash
   docker --version
   ```

6. Створіть файл `validator.env`:
   ```bash
   nano validator.env
   ```

7. Додайте наступні параметри до `validator.env`, замінивши їх на ваші дані:
   ```env
   ENV=testnet-3

   STRATEGY_EXECUTOR_IP_ADDRESS=Ваша_IP_адреса_VPS/VDS
   STRATEGY_EXECUTOR_DISPLAY_NAME=Назва_вашої_ноди
   STRATEGY_EXECUTOR_BENEFICIARY=Адреса_першого_гаманця_Elixir_v3
   SIGNER_PRIVATE_KEY=Private_key_другого_пустого_гаманця_elixir-empty
   ```

8. Збережіть файл, натиснувши `Ctrl + O`, підтвердіть натиснувши Enter, та поверніться до головного екрану, натиснувши `Ctrl + X`.

## 3. Мінт токенів

**Перш ніж почати, переконайтеся, що у вас вибраний перший гаманець "Elixir v3".**

1. Перейдіть на [Elixir Testnet](https://testnet-3.elixir.xyz/).
2. Підключіть гаманець "Elixir v3".
3. Мінтимо 1000 MOCK.
4. Підтвердіть токени MOCK першою транзакцією, а потім натисніть Stake другою транзакцією.
5. Контракт MOCK токена: `0x800eC0D65adb70f0B69B7Db052C6bd89C2406aC4`.
6. Делегуйте свої токени обраному валідатору або декільком.

## 4. Запуск валідатора

1. Завантажте та створіть образ:
   ```bash
   docker pull elixirprotocol/validator:v3
   ```

2. Запустіть Validator Node:
   ```bash
   docker run --env-file /root/validator.env --restart unless-stopped -p 17690:17690 elixirprotocol/validator:v3
   ```

3. Для перевірки, чи запустився контейнер з нодою:
   ```bash
   docker ps -a
   ```
   
4. Лог ноди:
   ```bash
   docker logs elixir -f -n 500
   ```

## 6. Оновлення ноди

1. Завершіть процес:
   ```bash
   docker kill elixir
   ```

2. Видаліть образ:
   ```bash
   docker rm elixir
   ```

3. Завантажте та створіть новий образ:
   ```bash
   docker pull elixirprotocol/validator:v3
   ```

4. Запускаємо Validator Node:
    ```bash
   docker run --env-file /root/validator.env --restart unless-stopped -p 17690:17690 elixirprotocol/validator:v3
   ```
