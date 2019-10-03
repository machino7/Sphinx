FROM python:3.7-slim-stretch

# javaのインストールのためにman1ディレクトリを作成
RUN mkdir /usr/share/man/man1

# ライブラリをインストール
RUN apt -y update \
    && apt -y install \
        openjdk-8-jre \
        graphviz \
        ttf-dejavu \
        python-dev \
        python-pip \
        git \
        make \
        wget \

# パッケージ掃除
    && apt clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# 環境変数
ENV \
    LIBRARY_PATH=/lib:/usr/lib \
    PLANTUML_DIR=/usr/local/plantuml \
    PLANTUML_JAR=plantuml.jar \
    PLANTUML=$PLANTUML_DIR/$PLANTUML_JAR

# plantUML
RUN \
    echo "#PLANTUML" && \
    mkdir $PLANTUML_DIR  && \
    wget "https://sourceforge.net/projects/plantuml/files/plantuml.jar" --no-check-certificate && \
    echo "#Check jar file size. tefs #9. normal jar file size is maybe > 5mb" && \
    size=$(stat -c %s plantuml.jar) && \
    test $size -gt 5000000 && \
    mv plantuml.jar $PLANTUML_DIR

# TakaoFont
RUN \
    echo "#TakaoFont for Japanese" && \
    wget "https://launchpad.net/takao-fonts/trunk/15.03/+download/TakaoFonts_00303.01.tar.xz" && \
    tar xvf TakaoFonts_00303.01.tar.xz -C /usr/share/fonts/ && \
    rm -f TakaoFonts_00303.01.tar.xz && \
    ln -s /usr/share/fonts/TakaoFonts_00303.01 /usr/share/fonts/TakaoFonts

# Install Sphinx theme&Extention
COPY requirements.txt ./
RUN \
    echo "#Upgrade pip" && \
    pip install --upgrade pip && \
    echo "#Install Sphinx with Nice theme&Extention" && \
    pip install -U -r requirements.txt && \
    pip freeze

COPY conf.py /usr/share/docker-conf.py

WORKDIR /media
CMD ["sphinx-autobuild", "-b", "html", "-H", "0.0.0.0", "/media/doc", "/tmp/html"]
