# ETL Pipeline

Project xử lý dữ liệu JSON bằng **Apache Spark**, lưu vào **MongoDB**, kết hợp **MySQL** và **Kafka** cho pipeline CDC.

## Luồng hoạt động

```
JSON → Spark (ETL) → MongoDB
MySQL → Kafka → Consumer
```

## Cấu trúc thư mục

```
ETL/
├── config/          # Cấu hình DB & Spark
├── database/        # Kết nối & tạo schema MySQL, MongoDB
├── data/            # File dữ liệu nguồn (sample_data.json)
├── lib/             # JAR connector cho Spark
├── sql/             # Script SQL
├── src/
│   ├── spark/       # Pipeline ETL chính
│   └── kafka/       # Kafka consumer
├── .env             # Thông tin kết nối (tự tạo, không commit)
└── requirements.txt
```

## Yêu cầu

- Python 3.10+
- Java 11+ (cho Spark)
- MySQL 8.0+
- MongoDB 4.4+
- Kafka (khi chạy consumer)

## Cài đặt

```bash
# 1. Tạo môi trường Python
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 2. Tạo file .env (xem mẫu bên dưới)

# 3. Đặt dữ liệu vào data/sample_data.json

# 4. Khởi tạo database
python database/mysql_connect.py
python database/mongodb_connect.py
```

### Mẫu file `.env`

```env
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_password
MYSQL_DATABASE=new_data
MYSQL_JAR_PATH=lib/mysql-connector-j-9.2.0.jar

MONGO_URI=mongodb://localhost:27017/
MONGO_DB_NAME=new_data
MONGO_PACKAGE_PATH=lib/mongo-spark-connector_2.13-10.5.0.jar
```

## Chạy project

```bash
# ETL: đọc JSON → transform → ghi MongoDB
python src/spark/spark_main.py

# Kafka consumer
python src/kafka/consumer.py
```

## Kiểm tra kết quả

```bash
# MongoDB
mongosh
use new_data
db.Users.find().limit(5)

# MySQL
mysql -u root -p
USE new_data;
SELECT * FROM Users LIMIT 5;
```
