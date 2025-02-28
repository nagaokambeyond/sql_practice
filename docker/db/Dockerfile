FROM postgres:17.0

# 日本語設定
RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG="ja_JP.UTF-8" \
    LANGUAGE="ja_JP:ja" \
    LC_ALL="ja_JP.UTF-8"

# 環境変数
ENV POSTGRES_USER="dvdrental"
ENV POSTGRES_PASSWORD="password"
ENV POSTGRES_DB="dvdrental"

# サンプルデータ取得
ENV ARCHIVE_NAME="dvdrental.zip"
ENV BACKUP_NAME="dvdrental.tar"
ENV URL="http://www.postgresqltutorial.com/wp-content/uploads/2019/05/${ARCHIVE_NAME}"

# サンプルデータ展開
RUN apt-get update && apt-get install -y wget unzip
RUN wget -nv "${URL}" -O "/tmp/${ARCHIVE_NAME}" && unzip -q "/tmp/${ARCHIVE_NAME}" -d /tmp

# 復元用スクリプトをコピー
COPY restoredb.sh /docker-entrypoint-initdb.d/
RUN chmod 755 /docker-entrypoint-initdb.d/restoredb.sh
