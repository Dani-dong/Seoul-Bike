{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": [],
      "authorship_tag": "ABX9TyMVK87+KMep5jwUmcGuYSol",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/Dani-dong/Seoul-Bike/blob/main/README.md\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 1,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "R6W4gNmN-nmE",
        "outputId": "2e782849-87fb-4290-dedd-ac7e9d367b01"
      },
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount(\"/content/drive\", force_remount=True).\n"
          ]
        }
      ],
      "source": [
        "# ━━━━ 매번 맨 처음에 실행 ━━━━\n",
        "\n",
        "from google.colab import drive\n",
        "drive.mount('/content/drive')\n",
        "\n",
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "import matplotlib.font_manager as fm\n",
        "import os\n",
        "\n",
        "# 나눔 폰트 설치 및 설정\n",
        "!apt-get -qq install fonts-nanum\n",
        "\n",
        "# 폰트 캐시 삭제 (한글 깨짐 방지 핵심)\n",
        "!rm -rf ~/.cache/matplotlib\n",
        "\n",
        "fe = fm.FontEntry(\n",
        "    fname=r'/usr/share/fonts/truetype/nanum/NanumGothic.ttf',\n",
        "    name='NanumGothic')\n",
        "fm.fontManager.ttflist.insert(0, fe)\n",
        "plt.rcParams.update({'font.size': 12, 'font.family': 'NanumGothic'})\n",
        "plt.rcParams['axes.unicode_minus'] = False\n",
        "\n",
        "PATH = '/content/drive/MyDrive/Colab Notebooks/04.14 실습_data/'"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df = pd.read_csv(PATH + '따릉이_분석용.csv', encoding='utf-8')\n",
        "# 한글 깨지면 encoding='euc-kr' 또는 'utf-8' 시도\n",
        "\n",
        "print(f'데이터 크기: {df.shape[0]:,}행 × {df.shape[1]}열')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Kv2yDd-G-2N5",
        "outputId": "edfa9b38-66e5-44e0-dd3d-6778c7b1aa6b"
      },
      "execution_count": 2,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "데이터 크기: 13,795행 × 8열\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "9JKr2F4T_4eG",
        "outputId": "bd0e5bec-7834-48f1-b209-53540f59e7ee"
      },
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   자치구               대여소명    기준년월  대여건수  반납건수         위도          경도  거치대수\n",
              "0  마포구       108. 서교동 사거리  202507  1277  1314  37.544582  127.044609  10.0\n",
              "1  양천구   729. 서부식자재마트 건너편  202507  1658  1808  37.481491  126.981583   NaN\n",
              "2  양천구  731. 서울시 도로환경관리센터  202507  3135  3218  37.486835  126.968048   NaN\n",
              "3  양천구   733. 신정이펜하우스314동  202507   876   375  37.494499  126.916527   NaN\n",
              "4  양천구      734. 신트리공원 입구  202507  2987  2958  37.688274  127.049065   NaN"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-4874cdfe-ba01-44e9-88c6-bc810894966f\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>자치구</th>\n",
              "      <th>대여소명</th>\n",
              "      <th>기준년월</th>\n",
              "      <th>대여건수</th>\n",
              "      <th>반납건수</th>\n",
              "      <th>위도</th>\n",
              "      <th>경도</th>\n",
              "      <th>거치대수</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>마포구</td>\n",
              "      <td>108. 서교동 사거리</td>\n",
              "      <td>202507</td>\n",
              "      <td>1277</td>\n",
              "      <td>1314</td>\n",
              "      <td>37.544582</td>\n",
              "      <td>127.044609</td>\n",
              "      <td>10.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>양천구</td>\n",
              "      <td>729. 서부식자재마트 건너편</td>\n",
              "      <td>202507</td>\n",
              "      <td>1658</td>\n",
              "      <td>1808</td>\n",
              "      <td>37.481491</td>\n",
              "      <td>126.981583</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>양천구</td>\n",
              "      <td>731. 서울시 도로환경관리센터</td>\n",
              "      <td>202507</td>\n",
              "      <td>3135</td>\n",
              "      <td>3218</td>\n",
              "      <td>37.486835</td>\n",
              "      <td>126.968048</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>양천구</td>\n",
              "      <td>733. 신정이펜하우스314동</td>\n",
              "      <td>202507</td>\n",
              "      <td>876</td>\n",
              "      <td>375</td>\n",
              "      <td>37.494499</td>\n",
              "      <td>126.916527</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>양천구</td>\n",
              "      <td>734. 신트리공원 입구</td>\n",
              "      <td>202507</td>\n",
              "      <td>2987</td>\n",
              "      <td>2958</td>\n",
              "      <td>37.688274</td>\n",
              "      <td>127.049065</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-4874cdfe-ba01-44e9-88c6-bc810894966f')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-4874cdfe-ba01-44e9-88c6-bc810894966f button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-4874cdfe-ba01-44e9-88c6-bc810894966f');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "df",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 13795,\n  \"fields\": [\n    {\n      \"column\": \"\\uc790\\uce58\\uad6c\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 25,\n        \"samples\": [\n          \"\\uc911\\ub791\\uad6c\",\n          \"\\uc911\\uad6c\",\n          \"\\ub9c8\\ud3ec\\uad6c\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub300\\uc5ec\\uc18c\\uba85\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2793,\n        \"samples\": [\n          \"1243. \\ubb38\\uc815 \\ubc95\\uc870\\ub2e8\\uc9c07\",\n          \"3763. \\ub4f1\\ucd0c\\ud0dc\\uc601\\uc544\\ud30c\\ud2b8\",\n          \"1955. \\ub514\\uc9c0\\ud138\\uc785\\uad6c \\uad50\\ucc28\\ub85c\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uae30\\uc900\\ub144\\uc6d4\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1,\n        \"min\": 202507,\n        \"max\": 202511,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          202508,\n          202511,\n          202509\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub300\\uc5ec\\uac74\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1168,\n        \"min\": 0,\n        \"max\": 17664,\n        \"num_unique_values\": 3573,\n        \"samples\": [\n          1494,\n          1456,\n          1597\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ubc18\\ub0a9\\uac74\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1217,\n        \"min\": 0,\n        \"max\": 18654,\n        \"num_unique_values\": 3624,\n        \"samples\": [\n          6224,\n          4121,\n          4471\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uc704\\ub3c4\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 6.349872914624114,\n        \"min\": 0.0,\n        \"max\": 37.692322,\n        \"num_unique_values\": 1718,\n        \"samples\": [\n          37.630016,\n          37.464298,\n          37.536999\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uacbd\\ub3c4\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 21.473882934787095,\n        \"min\": 0.0,\n        \"max\": 127.17701,\n        \"num_unique_values\": 1726,\n        \"samples\": [\n          126.826523,\n          127.092598,\n          126.934227\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uac70\\uce58\\ub300\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 6.01937115209632,\n        \"min\": 2.0,\n        \"max\": 62.0,\n        \"num_unique_values\": 42,\n        \"samples\": [\n          24.0,\n          7.0,\n          13.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 3
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.info()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "mleJy4slARb8",
        "outputId": "3c21bf01-e25b-4e48-b7dd-d04a94f73668"
      },
      "execution_count": 4,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 13795 entries, 0 to 13794\n",
            "Data columns (total 8 columns):\n",
            " #   Column  Non-Null Count  Dtype  \n",
            "---  ------  --------------  -----  \n",
            " 0   자치구     13795 non-null  object \n",
            " 1   대여소명    13795 non-null  object \n",
            " 2   기준년월    13795 non-null  int64  \n",
            " 3   대여건수    13795 non-null  int64  \n",
            " 4   반납건수    13795 non-null  int64  \n",
            " 5   위도      9030 non-null   float64\n",
            " 6   경도      9030 non-null   float64\n",
            " 7   거치대수    8532 non-null   float64\n",
            "dtypes: float64(3), int64(3), object(2)\n",
            "memory usage: 862.3+ KB\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.columns"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "7VIrjiRxAVhq",
        "outputId": "366a4eab-5656-40ac-b15a-57c9eb799312"
      },
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Index(['자치구', '대여소명', '기준년월', '대여건수', '반납건수', '위도', '경도', '거치대수'], dtype='object')"
            ]
          },
          "metadata": {},
          "execution_count": 5
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "print(df.shape)\n",
        "# 또는\n",
        "print(f'{len(df):,}개 대여소')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "5BRW-YScAytw",
        "outputId": "ad55f884-52c0-4be1-a65f-8cd5f360ae02"
      },
      "execution_count": 6,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(13795, 8)\n",
            "13,795개 대여소\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.head(10)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 363
        },
        "id": "xKN7ER4HBDGs",
        "outputId": "bcf3e8ce-2380-422e-f738-abe10af0707e"
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   자치구               대여소명    기준년월  대여건수  반납건수         위도          경도  거치대수\n",
              "0  마포구       108. 서교동 사거리  202507  1277  1314  37.544582  127.044609  10.0\n",
              "1  양천구   729. 서부식자재마트 건너편  202507  1658  1808  37.481491  126.981583   NaN\n",
              "2  양천구  731. 서울시 도로환경관리센터  202507  3135  3218  37.486835  126.968048   NaN\n",
              "3  양천구   733. 신정이펜하우스314동  202507   876   375  37.494499  126.916527   NaN\n",
              "4  양천구      734. 신트리공원 입구  202507  2987  2958  37.688274  127.049065   NaN\n",
              "5  양천구        735. 영도초등학교  202507  2687  3038  37.626614  127.072754   NaN\n",
              "6  양천구         736. 오솔길공원  202507  1069  1179  37.649021  127.076408   NaN\n",
              "7  양천구          737. 장수공원  202507   717   571  37.493729  127.120621   NaN\n",
              "8  양천구         739. 신월사거리  202507   950   960  37.512104  127.107780   NaN\n",
              "9  양천구          740. 으뜸공원  202507  1076  1322  37.502594  127.127647   NaN"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-9dd33a02-e1f9-49b8-b4dc-e7bf4ac9bf19\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>자치구</th>\n",
              "      <th>대여소명</th>\n",
              "      <th>기준년월</th>\n",
              "      <th>대여건수</th>\n",
              "      <th>반납건수</th>\n",
              "      <th>위도</th>\n",
              "      <th>경도</th>\n",
              "      <th>거치대수</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>마포구</td>\n",
              "      <td>108. 서교동 사거리</td>\n",
              "      <td>202507</td>\n",
              "      <td>1277</td>\n",
              "      <td>1314</td>\n",
              "      <td>37.544582</td>\n",
              "      <td>127.044609</td>\n",
              "      <td>10.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>양천구</td>\n",
              "      <td>729. 서부식자재마트 건너편</td>\n",
              "      <td>202507</td>\n",
              "      <td>1658</td>\n",
              "      <td>1808</td>\n",
              "      <td>37.481491</td>\n",
              "      <td>126.981583</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>양천구</td>\n",
              "      <td>731. 서울시 도로환경관리센터</td>\n",
              "      <td>202507</td>\n",
              "      <td>3135</td>\n",
              "      <td>3218</td>\n",
              "      <td>37.486835</td>\n",
              "      <td>126.968048</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>양천구</td>\n",
              "      <td>733. 신정이펜하우스314동</td>\n",
              "      <td>202507</td>\n",
              "      <td>876</td>\n",
              "      <td>375</td>\n",
              "      <td>37.494499</td>\n",
              "      <td>126.916527</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>양천구</td>\n",
              "      <td>734. 신트리공원 입구</td>\n",
              "      <td>202507</td>\n",
              "      <td>2987</td>\n",
              "      <td>2958</td>\n",
              "      <td>37.688274</td>\n",
              "      <td>127.049065</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>5</th>\n",
              "      <td>양천구</td>\n",
              "      <td>735. 영도초등학교</td>\n",
              "      <td>202507</td>\n",
              "      <td>2687</td>\n",
              "      <td>3038</td>\n",
              "      <td>37.626614</td>\n",
              "      <td>127.072754</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>6</th>\n",
              "      <td>양천구</td>\n",
              "      <td>736. 오솔길공원</td>\n",
              "      <td>202507</td>\n",
              "      <td>1069</td>\n",
              "      <td>1179</td>\n",
              "      <td>37.649021</td>\n",
              "      <td>127.076408</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>7</th>\n",
              "      <td>양천구</td>\n",
              "      <td>737. 장수공원</td>\n",
              "      <td>202507</td>\n",
              "      <td>717</td>\n",
              "      <td>571</td>\n",
              "      <td>37.493729</td>\n",
              "      <td>127.120621</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>8</th>\n",
              "      <td>양천구</td>\n",
              "      <td>739. 신월사거리</td>\n",
              "      <td>202507</td>\n",
              "      <td>950</td>\n",
              "      <td>960</td>\n",
              "      <td>37.512104</td>\n",
              "      <td>127.107780</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9</th>\n",
              "      <td>양천구</td>\n",
              "      <td>740. 으뜸공원</td>\n",
              "      <td>202507</td>\n",
              "      <td>1076</td>\n",
              "      <td>1322</td>\n",
              "      <td>37.502594</td>\n",
              "      <td>127.127647</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-9dd33a02-e1f9-49b8-b4dc-e7bf4ac9bf19')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-9dd33a02-e1f9-49b8-b4dc-e7bf4ac9bf19 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-9dd33a02-e1f9-49b8-b4dc-e7bf4ac9bf19');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "df",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 13795,\n  \"fields\": [\n    {\n      \"column\": \"\\uc790\\uce58\\uad6c\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 25,\n        \"samples\": [\n          \"\\uc911\\ub791\\uad6c\",\n          \"\\uc911\\uad6c\",\n          \"\\ub9c8\\ud3ec\\uad6c\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub300\\uc5ec\\uc18c\\uba85\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2793,\n        \"samples\": [\n          \"1243. \\ubb38\\uc815 \\ubc95\\uc870\\ub2e8\\uc9c07\",\n          \"3763. \\ub4f1\\ucd0c\\ud0dc\\uc601\\uc544\\ud30c\\ud2b8\",\n          \"1955. \\ub514\\uc9c0\\ud138\\uc785\\uad6c \\uad50\\ucc28\\ub85c\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uae30\\uc900\\ub144\\uc6d4\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1,\n        \"min\": 202507,\n        \"max\": 202511,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          202508,\n          202511,\n          202509\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub300\\uc5ec\\uac74\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1168,\n        \"min\": 0,\n        \"max\": 17664,\n        \"num_unique_values\": 3573,\n        \"samples\": [\n          1494,\n          1456,\n          1597\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ubc18\\ub0a9\\uac74\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1217,\n        \"min\": 0,\n        \"max\": 18654,\n        \"num_unique_values\": 3624,\n        \"samples\": [\n          6224,\n          4121,\n          4471\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uc704\\ub3c4\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 6.349872914624114,\n        \"min\": 0.0,\n        \"max\": 37.692322,\n        \"num_unique_values\": 1718,\n        \"samples\": [\n          37.630016,\n          37.464298,\n          37.536999\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uacbd\\ub3c4\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 21.473882934787095,\n        \"min\": 0.0,\n        \"max\": 127.17701,\n        \"num_unique_values\": 1726,\n        \"samples\": [\n          126.826523,\n          127.092598,\n          126.934227\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uac70\\uce58\\ub300\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 6.01937115209632,\n        \"min\": 2.0,\n        \"max\": 62.0,\n        \"num_unique_values\": 42,\n        \"samples\": [\n          24.0,\n          7.0,\n          13.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 7
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.isnull().sum()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 335
        },
        "id": "BVaGyjcABGAN",
        "outputId": "f594578d-675e-4f69-9426-bf5cfd8fed67"
      },
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "자치구        0\n",
              "대여소명       0\n",
              "기준년월       0\n",
              "대여건수       0\n",
              "반납건수       0\n",
              "위도      4765\n",
              "경도      4765\n",
              "거치대수    5263\n",
              "dtype: int64"
            ],
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>0</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>자치구</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>대여소명</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>기준년월</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>대여건수</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>반납건수</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>위도</th>\n",
              "      <td>4765</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>경도</th>\n",
              "      <td>4765</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>거치대수</th>\n",
              "      <td>5263</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div><br><label><b>dtype:</b> int64</label>"
            ]
          },
          "metadata": {},
          "execution_count": 8
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.nlargest(5, '대여건수')[['대여소명', '자치구', '대여건수']]\n",
        "# 또는\n",
        "df.sort_values('대여건수', ascending=False).head(5)[['대여소명', '자치구', '대여건수']]"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "BLZnnsbJBI6q",
        "outputId": "3116c07c-8063-4386-989b-95f9bf4a3f21"
      },
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "                    대여소명  자치구   대여건수\n",
              "6383   2715.마곡나루역 2번 출구   강서구  17664\n",
              "889    2715.마곡나루역 2번 출구   강서구  15460\n",
              "3633   2715.마곡나루역 2번 출구   강서구  15117\n",
              "9140   2715.마곡나루역 2번 출구   강서구  14782\n",
              "11906  2715.마곡나루역 2번 출구   강서구  14217"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-d06c95a4-779e-4686-85e2-db5e6157af92\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>대여소명</th>\n",
              "      <th>자치구</th>\n",
              "      <th>대여건수</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>6383</th>\n",
              "      <td>2715.마곡나루역 2번 출구</td>\n",
              "      <td>강서구</td>\n",
              "      <td>17664</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>889</th>\n",
              "      <td>2715.마곡나루역 2번 출구</td>\n",
              "      <td>강서구</td>\n",
              "      <td>15460</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3633</th>\n",
              "      <td>2715.마곡나루역 2번 출구</td>\n",
              "      <td>강서구</td>\n",
              "      <td>15117</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>9140</th>\n",
              "      <td>2715.마곡나루역 2번 출구</td>\n",
              "      <td>강서구</td>\n",
              "      <td>14782</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>11906</th>\n",
              "      <td>2715.마곡나루역 2번 출구</td>\n",
              "      <td>강서구</td>\n",
              "      <td>14217</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-d06c95a4-779e-4686-85e2-db5e6157af92')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-d06c95a4-779e-4686-85e2-db5e6157af92 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-d06c95a4-779e-4686-85e2-db5e6157af92');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 5,\n  \"fields\": [\n    {\n      \"column\": \"\\ub300\\uc5ec\\uc18c\\uba85\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 1,\n        \"samples\": [\n          \"2715.\\ub9c8\\uace1\\ub098\\ub8e8\\uc5ed 2\\ubc88 \\ucd9c\\uad6c \"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uc790\\uce58\\uad6c\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 1,\n        \"samples\": [\n          \"\\uac15\\uc11c\\uad6c\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub300\\uc5ec\\uac74\\uc218\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1320,\n        \"min\": 14217,\n        \"max\": 17664,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          15460\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 9
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "영등포 = df[df['자치구'] == '영등포구']\n",
        "print(f'영등포구 대여소: {len(영등포)}개')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "sM8PI36yBMpt",
        "outputId": "9682d1ee-0361-413e-b2cd-27f5382554b0"
      },
      "execution_count": 10,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "영등포구 대여소: 849개\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "거치대없음 = df[df['거치대수'] == 0]\n",
        "print(f'{len(거치대없음)}개')\n",
        "거치대없음[['대여소명', '자치구']]"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 71
        },
        "id": "m-qb8fj_BPrE",
        "outputId": "af537f68-5678-4bf6-c369-416534b48438"
      },
      "execution_count": 11,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0개\n"
          ]
        },
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Empty DataFrame\n",
              "Columns: [대여소명, 자치구]\n",
              "Index: []"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-8334b0d2-e40a-4cac-99c7-382dfe49c3bb\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>대여소명</th>\n",
              "      <th>자치구</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-8334b0d2-e40a-4cac-99c7-382dfe49c3bb')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-8334b0d2-e40a-4cac-99c7-382dfe49c3bb button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-8334b0d2-e40a-4cac-99c7-382dfe49c3bb');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "repr_error": "Out of range float values are not JSON compliant: nan"
            }
          },
          "metadata": {},
          "execution_count": 11
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.groupby('자치구').size().sort_values(ascending=False)\n",
        "# 또는\n",
        "df['자치구'].value_counts()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 899
        },
        "id": "lF5Sw9jpBSQa",
        "outputId": "4f729909-7af9-474a-8145-aac55fb33f1e"
      },
      "execution_count": 13,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "자치구\n",
              "송파구     1096\n",
              "강서구      966\n",
              "강남구      851\n",
              "영등포구     849\n",
              "노원구      755\n",
              "서초구      723\n",
              "마포구      601\n",
              "강동구      601\n",
              "구로구      574\n",
              "양천구      548\n",
              "종로구      496\n",
              "은평구      480\n",
              "중랑구      479\n",
              "성동구      473\n",
              "중구       457\n",
              "동대문구     446\n",
              "용산구      435\n",
              "광진구      434\n",
              "성북구      404\n",
              "서대문구     403\n",
              "도봉구      372\n",
              "금천구      361\n",
              "동작구      354\n",
              "관악구      344\n",
              "강북구      293\n",
              "Name: count, dtype: int64"
            ],
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>count</th>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>자치구</th>\n",
              "      <th></th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>송파구</th>\n",
              "      <td>1096</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>강서구</th>\n",
              "      <td>966</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>강남구</th>\n",
              "      <td>851</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>영등포구</th>\n",
              "      <td>849</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>노원구</th>\n",
              "      <td>755</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>서초구</th>\n",
              "      <td>723</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>마포구</th>\n",
              "      <td>601</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>강동구</th>\n",
              "      <td>601</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>구로구</th>\n",
              "      <td>574</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>양천구</th>\n",
              "      <td>548</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>종로구</th>\n",
              "      <td>496</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>은평구</th>\n",
              "      <td>480</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>중랑구</th>\n",
              "      <td>479</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>성동구</th>\n",
              "      <td>473</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>중구</th>\n",
              "      <td>457</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>동대문구</th>\n",
              "      <td>446</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>용산구</th>\n",
              "      <td>435</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>광진구</th>\n",
              "      <td>434</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>성북구</th>\n",
              "      <td>404</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>서대문구</th>\n",
              "      <td>403</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>도봉구</th>\n",
              "      <td>372</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>금천구</th>\n",
              "      <td>361</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>동작구</th>\n",
              "      <td>354</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>관악구</th>\n",
              "      <td>344</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>강북구</th>\n",
              "      <td>293</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div><br><label><b>dtype:</b> int64</label>"
            ]
          },
          "metadata": {},
          "execution_count": 13
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "gu_rent = df.groupby('자치구')['대여건수'].sum().sort_values(ascending=False)\n",
        "print(gu_rent)\n",
        "print(f'\\n1위: {gu_rent.index[0]} ({gu_rent.iloc[0]:,}건)')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "2iAMzvthBWPp",
        "outputId": "6500f149-43cd-4bd6-9ee0-9d0b4981a592"
      },
      "execution_count": 14,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "자치구\n",
            "강서구     2200391\n",
            "송파구     1644794\n",
            "영등포구    1630819\n",
            "양천구     1095806\n",
            "노원구     1046746\n",
            "강동구      844581\n",
            "마포구      838684\n",
            "광진구      838194\n",
            "구로구      725741\n",
            "성동구      656857\n",
            "강남구      611453\n",
            "동대문구     582291\n",
            "종로구      569590\n",
            "서초구      535191\n",
            "중랑구      478850\n",
            "중구       418676\n",
            "은평구      404271\n",
            "도봉구      371256\n",
            "성북구      356768\n",
            "용산구      350394\n",
            "관악구      345402\n",
            "서대문구     321144\n",
            "동작구      306411\n",
            "금천구      291549\n",
            "강북구      216729\n",
            "Name: 대여건수, dtype: int64\n",
            "\n",
            "1위: 강서구 (2,200,391건)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import matplotlib.pyplot as plt\n",
        "\n",
        "# 이미 첫 번째 셀에서 설정했으므로 바로 그립니다.\n",
        "gu_rent = df.groupby('자치구')['대여건수'].sum().sort_values(ascending=False)\n",
        "\n",
        "plt.figure(figsize=(14, 6))\n",
        "gu_rent.plot(kind='bar', color='#00B050')\n",
        "plt.title('자치구별 따릉이 총 대여건수', fontsize=16)\n",
        "plt.xlabel('자치구')\n",
        "plt.ylabel('대여건수')\n",
        "plt.xticks(rotation=45, ha='right')\n",
        "plt.tight_layout()\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 354
        },
        "id": "o3WSMalsCJoT",
        "outputId": "e59424cd-b5e9-4e22-bf73-141e64ca6950"
      },
      "execution_count": 15,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 1400x600 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAABWgAAAJICAYAAAD8eA38AAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAPYQAAD2EBqD+naQAApyxJREFUeJzs3Xd4FOXXxvEzqYSS0DtKk44KiKDSOygBFBEQrEhHqiCKCirSBBQQkCKgAkFqQJAmoSkQDUjvRUJvSijpOe8feXd+2TTSyGzW7+e6uEhmZ3efkyk7c+8zzxiqqgIAAAAAAAAAyHQuVjcAAAAAAAAAAP6rCGgBAAAAAAAAwCIEtAAAAAAAAABgEQJaAAAAAAAAALAIAS0AAAAAAAAAWISAFgAAAAAAAAAsQkALAAAAAAAAABYhoAUAAAAAAAAAi7hZ3QAAAABndPv2bTl9+rQ8+eST4uKScd+Jnz17VlRVSpcunWGvmZVduHBB7t27J+XLl7e6KcmKiYmRv/76S8qUKSM+Pj5pfp3Q0FA5cuSIqKrddMMw5PHHHxd3d/f0NvWBYmJi5N9//5W8efOKiIiqyr59++Sxxx6TXLlyZch77N+/X4oWLSoFChTIkNdLj6CgIClRooQULFjQ6qYAAAAnRQ9aAACAh8DPz09q1KghZ8+eTXa+w4cPy19//WX378CBAxITE5Po/G+99Za8+uqrSb7e+fPn5dixYyn+FxwcnCDsi6tOnTry/vvvJ/l4/fr15b333ksw3cPDQz755JNkKs8Y7733nrRo0SLZeT799FMxDCPF/3Lnzi2LFi3K0HYGBwdLjRo1xM/PL12vExAQIE899ZTUrFnT7t9TTz0lf/31V8Y09gGWL18u+fLlk+DgYBERuXbtmtSoUUNWrFiRIa8fHh4uTz75pEyZMiVDXk9E5OLFi+Li4iLz5s1L1fNCQ0OlVq1aMmnSpAxrCwAAQHz0oAUAAHiAcePGyfXr1xN9zMXFRYoWLSrdu3eX7Nmzm9NtoWdy4WdERIRUqVIl0cfOnDkjpUqVSjBdVZN9zYYNG8qZM2eSfDwxb7zxRpLB1blz56Rs2bJJPvfvv/+WEiVKJJgeGRkp4eHhKW7D/fv3Zc6cOXL58mWzvmLFikmnTp1k3rx5cuvWLXN67ty5zdD4QX8PEZH+/ftLhw4dUtSO6OhoadasmaxatUo6d+6c7Lw///yzzJw5U44dOybZsmWT2rVry3vvvZdob96UrA8p0bRpUzl16lSiPWhLliyZrteeNWuW7N27V6KiohI8ljdvXhk/fryIxP6N4v7/oNpiYmJk9uzZsmjRIjl//rwUKlRIGjVqJEOGDDF74cb1oNcbNGiQHDlyJNHHDMOQggULykcffWS33kZGRoqqpmqdFBFZs2aNREdHS44cOVL1PAAAgNQgoAUAAHiAvXv3ysWLFxN97NSpU3L16lUpX768tGzZMlWv6+HhIadOnZLIyEi76S4uLmkO206fPp2q+Tt37iy//vprmt7LJioqSu7evZuu11i4cKH0799fGjZsKB4eHiIiEhISIpMnT5bx48dLw4YNzaEi8ufPn6rX9vHxSdWwArly5ZKIiIhk53n//fdl3Lhx0rx5c+nTp4/cv39fFixYIH5+frJixQpp1qxZqtqYnMjISDlz5swDw91Tp06Jj4+PFC5cONXvcfnyZenRo4eUK1cu0S8GwsLCUv2aIrHrRuvWrWX9+vXSqFEj6dChg1y8eFEmTZokCxculK1btyb6fsnJnz9/kjVeuHBBvv/+eylevLiMHj06TW22uX//vnz00UdiGIZ8/fXX0qNHD4Y5AAAADwUBLQAAwAMsWbIkycf69+8v33zzjTz55JNpeu0yZcqksVUZw9vb+4HBX0REhPz777+JPhYTEyNLlixJ9m+UEjdv3hQRkS1btthN79mzpxQrVkw2btyYrte/deuWXL9+/YG1xsTEyN27d8XT0zPJedauXSvjxo2T999/X8aMGWNOHzhwoDRr1kw6dOggJ06cyLAwb9++fVKrVq0Uz9+mTRtZtWpVqt7j9u3bIiIyfPhweeONN1L13OSMHj1a1q9fLzNnzpQePXqY04cNGyYNGjSQjh07yu7du8UwjBS/5gcffJDkY3v27JFff/1VypUrl652q6q8/fbbcvr0aVm1apX06dNHfH195ZdffpE8efKk67UBAADiI6AFAABIo5iYGFm5cqW0aNFCihQpkqrndu3aVf74449EHzMMQwoUKCCTJ0+WGjVqZERTk3T79m27oRkSs3jxYlm8eHGSjzdq1CjBOLSp7U38MK1YsUJefvnlJMf1jS9PnjzSrl27JB8fM2aMFC1aVD777DO76dmzZ5e5c+dKxYoVZeLEiTJu3Lh0tdvm6aefTjZY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afaSiYyMzFL77QULFuiFCxe0cOHCGhgYqP3799eKFSvqvHnzVDV2u8qTJ4/++uuvqpp0PRERETp06FC7oa8y02+//aaGYejp06f122+/VVdXV925c6fOmDFDvby89ODBg6qadT5X424j3333nbq5uamvr682bdpUN27cqKr/239ltc8g1dia3N3d1dfXV5s0aaLr16832x4UFKR//PFHkvttG0eoSdX+c2TIkCHaqlUr/eyzz9TT01MnTJigqrHLKH6vwPgcpZ74+vfvr5UqVdJcuXKZ23dW2885y7HC0aNHE1wBNWjQoCy7fJIaim/ChAnq5eWlf/31l6pmnf22qv1597Bhw9QwDO3fv3+CnrQXL17UPHny6NKlS1XVcY+30+I/H9AmtoLGxMRosWLFtFOnTua0u3fvmitr/OfEXRE2b978wDEeH4a4bXrhhRe0YsWKdo9PnDhR27Ztq+XKldNKlSrZPZbcnRhVrb3UyvbBsGLFCt28ebOWK1dOJ02alOhl55GRkeY3/4ktV1sdV69etayHwt9//23esTf+37V48eJZap1Tje3Z3LNnT23UqJG2aNFCP//8c7vHhw4dqp6enrpnzx5VTXh34/isXueSC5VOnjypGzduNNt4//59M5R21OWj+r+apk+frj///LP6+vrqsGHDzL9x3G8bs8I6Z+vpYgsu4rctqZ60jr7OTZ8+3W56//79tUCBAnr79m276YkdgMRldT2qyW9He/bs0fLly5thUmRkpH766aeqmvQBoyPUZGNry4EDB7RZs2bq4+Ojv/zyi/n4/fv3HX4bSm75DBo0SEuVKqWqsT0TsmXLZp5IJbXOWb1cnHEb2rBhg3p6euq6det01apV6uHhoRMnTtQSJUrYndheu3bNHMJK1XFrsu23c+fOrXPnzjWPg1RVR40apfXr19dffvklwRe9jlqPbZ2z9ZwfP368enl56TvvvKPNmjXT4OBgVVVdvHixuri46Jo1a+za7Wh27NihOXLk0OrVq6tqbO9Z2+fr3r171TAMzZkzZ5Y5lovbrunTp6ubm5t5TNC8eXNt0qSJ+SVhVvkMSq6mxo0bm1d/Llq0SPPnz28G6o66jGyXvtvaEfdvP2zYMC1UqJB26dLFnObo65xqbOeBGTNm6BtvvKFz587VzZs3m499/PHH2qBBA12/fr25n4s/bn18Vtfk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4h7nONJ+OyP85wJa2wK8efOmDhw4UDt27Gj3LYmtF1ncAYovXLjgsCGtqmrt2rXVzc1Nhw8fbnf53vXr17V79+52Ie2RI0f09OnT+uWXX6qvr69OmDDBoXrS2nr/zp49W8PDw7VLly5as2ZNvXjxoqqqGbpu3rxZXV1dzfF9oqKizGWyatUqrVatmhnsOoKQkBBdvXq1urq6mjcnaNWqld0HRnh4uB49elRVY3t5ly1b1jwodjTLly/X4sWL671798xpu3fv1tKlS+uFCxf0p59+0kKFCunff/+tU6dOVS8vLz169KiOGjVK8+TJY9ng8Ik5cuSINm3aVN9//327HjB//vmn5suXT0uWLGlOK1GihN2lcI7WyycmJkajo6O1Tp06umjRIr13756WKVNGu3fvbg4JYjuImj9/vrq6uur69etV1X4bWrt2rVapUkX//vtvawqJZ/fu3WY7VWP/7rdv39ZChQpp8eLFtUuXLtq6dWt99913zW9VbRYvXmz2onUk8S+1tgVLcYNxWy/Tdu3a2c27du1aNQzDPJh0FEePHtWePXtq27Ztzf2cbViDypUra7FixXT//v1apkwZ7dmzp6rG9vQeOXKkrl69OsGwPFa5ceOGNm3aVMeNG2dOi3/yceLECe3Ro4cOHTrUnHbz5k2dNm2auR9XdYzPVZuoqCht0qSJNm3a1BwWaM+ePeYJ16FDh8xeGarJ798coS5n3IZUY3so5cqVK8Hle5UrV9aWLVvqvn37zJOw+PttR6rpu+++My97jYqK0oCAAH3ttdf0rbfeMscFHjFihHbo0MEM+0JCQnTjxo06efJk9ff3t/uixFHEXfenTJmiJUuWNPddixcvVsMwdPDgwRoREaELFy7UwoULq4uLi3p7e+vTTz/tEIHM5cuXtWTJkjp69GidO3euPvXUU7po0SJVja1v6dKl2r17d61du7aWKFFCjx07pkuXLtVWrVppxYoVtW7dumoYhm7atMniSuwFBQXpwIEDNUeOHOYVeIGBgerh4aGVKlXKMp9BccWtyXbDrJ07d5rDhhw9elQNwzC/EDhx4oTmypVLn3rqKf32228TjMFttbjbz9tvv62GYWj+/Pn19ddf15dffln79eunzz//vF2v+3fffVcnTJjgsJedT506VR977LFEx/h8+eWX9ZVXXlHV/11Of/z4cc2dO7dWq1ZNFy1a5HC9g53pWGHRokW6YsUK8/eYmBg9deqUZs+eXR999FHt0qWLvvrqq7ps2TLzBmFHjhzRXLly6RNPPOFwy8e2jn3zzTdqGIb5hc3t27e1VKlSOmHCBN24caPOnTtX27VrZ9d7e+bMmfr9999b0u7krFy5Ul944QWtV6+e3RWdtnB2yZIl5hcCts+pv//+W6dNm2Y3lrDV61pG+s8FtKr/W4AXL17UunXratu2bc00Pv48NhcuXNCKFSs6XEj79ddfa9u2bXXLli3auXNnHThwoN3Kev36de3Ro4d5U5ZPP/1UZ8yYoeHh4Tpp0iT19fXVL7/80uxJ6wjiDs59/PhxM6Q9f/68qsaGg3GHBIiMjDS/IVq7dq26urrqd999Z03j44kbhsXdkaqqdu/eXStWrKhffvmltm/fXqtUqaJjx441Q+h//vlHV61aZa6LjnBQb3Pz5k1t3769du/eXVevXq0//PCDFihQQP38/PTQoUOaJ08e/fnnn/XPP//UAgUKmHeRtB34h4SE6K5du/TcuXMWVxK7jObNm6ft2rUze1ccP35cPTw89N1339UqVaroiy++qKVKldJevXppTEyMfvvtt9qqVSvt1q2bQ467FPcb/KNHj2qZMmW0R48e5kn9unXr1DAMMwAICwtLcJL/448/Znq7UyLul2f16tUzL82MP09iNmzYYPllZXGXjc0nn3yi2bNntzuRCg8P1zp16th95qj+bx9nWz5WH5DEDy7//PNP7du3r7700ks6ceJEzZEjhzmcULdu3dQwDB04cKCGhobq888/r82bN9cKFSpovXr1EnwOW+XmzZv69ddfa926de3Gj4tb6+jRo7VZs2Zmb4SDBw9q9uzZNVu2bNqjR48khwyxiu0EKjIyUlu0aKEtW7bUadOmqYuLi13PhEceecTuio7o6Gi7dcwReqMn1lMnK29D8Y8ljx49an6pbttf+Pv7a/ny5c3w2XaDI9vxgqPVFN+2bdu0a9eu5qXzqrHh5uOPP667d+9WVdXTp09rvXr1tEuXLlqtWjVt2bJlgi/cHElQUJC+8sor5mfQ7Nmz1cXFRTt27KgNGjTQ5s2bq6urqzZr1kyXLFmigYGBWq1aNXNICivdvn3b3JYvXLig48aN05o1a+rixYvNeQYMGKA5cuTQU6dO6fz587VQoULq7+9vhhjDhg3TV155JdHPtMwWNyA6fPiwDhgwQDt37qwTJkzQ7NmzZ7nPoPj7OFtNnTp10i+++EI9PT3thjUwDENVY3sG58qVS99//30dP368tmzZUufOnZvg5ntWs9V34cIFrV27tvbv31+XLVumgwYN0pEjR+q0adMSLIu4PQMdZSxx27q/fft2fe655/TQoUNmbbZzttdee838skD1fwH6xx9/rKNGjdKmTZs61BAOznSsEJ+tfVeuXNEyZcokOizVoUOHNFeuXPrJJ5/op59+6lDLx7ZuXb9+XV9++WWz52x0dLTev39fixQpok8++aRWrVpVX3/9dR00aFCS4fnatWtTfP+ehy0qKkqXLVum9erV0zZt2qhq7BWstisEjhw5YveFwJEjRzR79uxaoEABffPNN3X79u3maznasU9a/acC2rgfeLafjxw5orlz51bDMLRBgwY6cOBAXbBgga5cuVL//PNPu16YFy5c0CpVqmjNmjUzve1JiXtXOn9/f23fvr0OHjw4QUjbs2dPLVy4sLZv397uG++vvvpKGzdurLNmzbL88oSk3v/EiRPapUsXbdCggU6dOlUNw9BatWrp0KFD7er8+eef1cXFxaFOUCIiIjQ6Olp/+OEH84PA9oHesWNHrVChgr7xxhv61Vdf6Q8//JDk68ycOVMnTZpk+TKyiY6OVn9/f+3WrZtWr15d33zzTd28ebMGBwdr5cqV9f3331fV2HUv7hAVNl9++aXmzJnTvNu5VWzLIioqSr///ntt06aNtmzZUl1dXfWLL75Q1dhL4wzD0BEjRui9e/e0du3a+tJLL+mgQYO0ffv22rVrV4f4ssYmMjLSXPdtf3NbSPv++++bA61nz55de/ToYffctWvXOsw2dPbsWTN8SOrvW6VKFXO7iTs8g03c9n/++eeaM2dOy74UuHXrlllH3HquX7+uLVq00OHDh5sH9LZLsvPmzatFihQxT0b8/f0TLB8rl9Hx48f1+++/T9DrKDAwUPv166c+Pj46duxYVY2tqXjx4vrBBx/oxYsXtUiRItqjRw/z0sannnrKrjeq1a5fv64zZszQZ555xryhjM3UqVO1VKlS5iXmhw4d0pw5c+r48eP12rVr2rRpU+3evbseO3bMgpb/T3BwsN0JSNwTr+eff17z5cunq1atMk/yFyxYoKtWrdK6devqyJEjE7ze2LFjtVGj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Z4kcEBO09Idq27atTJ8+XZKTkyU/P1+uXLki//rXv+Tf//63NoEYERERVe3y5cvyzDPPyNNPPy0iIgcPHpQ9e/bIPffcI7m5ubJmzRp5++23Zfv27dKlSxeDW0tERERGcHFxkfvuu0+Ki4vl66+/lvbt2zNJS+RgmKClP1SvXr1k+fLlkpKSIt7e3tK5c2fZtm2bNGvWzOimERERmV69evW05KzNZpPWrVtLbGysRERESFhYmNSqVUv27t0rISEhBreUiIiIjFRcXCzBwcHi7u7O5CyRA2INWrojrl69Kt7e3kY3g4iIyOEVFxdLTk6OBAQEiKenp/j4+BjdJCIiIjKBU6dOiZ+fn1gsFqObQkS/ERO0dEcAEEVRtP8SERERERERUfVT69QTkeNggpaIiIiIiIiIiIjIILylQkRERERERERERGQQJmiJiIiIiIiIiIiIDMIELREREREREREREZFBmKAlIiIiIiIiIiIiMggTtEREREREREREREQGYYKWiIiIiIiIiIiIyCBM0BIRERGRU7PZbJVeAHTLpKamSvfu3St9d926daIoiu61Z88e3TKnTp0ST0/PSt/Nzs6u9N2qXh07dqzeFSYiIiIih8IELRERERE5rXfeeUfc3NwqvTw8PCQzM1Nb7tq1a3Lt2rVK33/88cfl6tWrutfDDz+sW+b69etVfrdDhw5is9mkpKTkhq/Tp09Ldna2XL16tfpXnoiIiIgcAhO0REREROS0xo4dKwB0r6KiInFxcZE//elPv/p9i8UiXl5eutdv4eLiIq6urjd81ahRQ/t3iIiIiOjuxEiQiIiIiO4qGzdulAYNGkhkZORNl4uLi6uyJEHdunXl+PHj1dKWS5cuSc2aNeWee+6plt9HRERERI6HCVoiIiIiuqssW7ZMxo4dK4qi3HS5lStXVipJsGPHDrFarVKvXr1qact3330nTZs2rZbfRURERESOydXoBhARERER3SkrV66UU6dOSXx8/K8uqyiKuLr+Ei4DkBkzZshTTz11S+URbkVubq6EhoZWy+8iIiIiIsfEEbREREREdFc4ffq0JCQkSIsWLcTT07PS57t379bKGGRlZVX6fMmSJfL999/LnDlzqq1N+/fvl5CQkGr7fURERETkeJigJSIiIiKn99NPP0nfvn0lJiZGTp8+LfPnz6+0zMMPPyzFxcVSXFwsvXr10n22ceNGmTJlivj7+1eZ3LW3b9++KmvXVvVKSUmRadOmaT+npaVV63oTERERkfkxQUtERERETu2HH36QHj16SHBwsCQlJcmmTZtk6dKlsnLlSt1yiqKIp6dnpQTs8uXLZdSoUbJ161b585//LP3795fCwsIb/nthYWGVateqr8mTJ8vo0aNv+HlMTMwf8ScgIiIiIhNjgpaIiIiInNaXX34pYWFh0qFDB1m9erVYLBZp3ry5bNu2TaZOnSqvvfbaDb975swZGTp0qMybN0927Ngh3bt3l7S0NGnQoIF06tRJDhw4cMPvurq6VvlSR8re6HMiIiIiuvswQUtERERETun8+fMSGRkpzz33nLzzzjvi4uKifda6dWvZu3evuLu7V/ndr776SkJCQqRRo0aSm5srrVu3FpHyxOvq1avlqaeeksTExDuyHkRERETk3HibnoiIiIicUsOGDSUvL08aNGhQ5edNmjSRyZMnV/lZy5Yt5cCBAxIYGFjl5/Hx8dXWTiIiIiK6u3EELRERERE5rRslZ3+Noig3TM4SEREREVUnJmiJiIiIiIiIiIiIDMIELRERERHd9Tw8PMTDw+O2vuvu7n7L33Vzc+NkYERERESkowCA0Y0gIiIiIiIiIiIiuhtxBC0RERERERERERGRQZigJSIiIiIiIiIiIjIIE7REREREREREREREBmGCloiIiIiIiIiIiMggTNASERERERERERERGYQJWiIiIiIiIiIiIiKDMEFLREREREREREREZBAmaImIiIiIiIiIiIgMwgQtERERERERERERkUGYoCUiIiIiIiIiIiIyyP8DfY9KdrmYihQAAAAASUVORK5CYII=\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "gu_balance = df.groupby('자치구').agg(\n",
        "    총대여=('대여건수', 'sum'),\n",
        "    총반납=('반납건수', 'sum')\n",
        ").reset_index()\n",
        "\n",
        "gu_balance['차이'] = gu_balance['총대여'] - gu_balance['총반납']\n",
        "\n",
        "print(gu_balance.sort_values('차이', ascending=False))"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "_FUUD0eFBcZo",
        "outputId": "8870615e-7dfe-4f12-de9d-29ba2bdcf576"
      },
      "execution_count": 16,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "     자치구      총대여      총반납     차이\n",
            "22   종로구   569590   544650  24940\n",
            "0    강남구   611453   586722  24731\n",
            "19  영등포구  1630819  1608719  22100\n",
            "23    중구   418676   400111  18565\n",
            "5    광진구   838194   824573  13621\n",
            "13  서대문구   321144   308351  12793\n",
            "3    강서구  2200391  2191002   9389\n",
            "17   송파구  1644794  1638586   6208\n",
            "14   서초구   535191   529950   5241\n",
            "20   용산구   350394   345910   4484\n",
            "4    관악구   345402   341729   3673\n",
            "2    강북구   216729   213422   3307\n",
            "6    구로구   725741   723587   2154\n",
            "9    도봉구   371256   370722    534\n",
            "7    금천구   291549   291172    377\n",
            "12   마포구   838684   838362    322\n",
            "8    노원구  1046746  1046468    278\n",
            "16   성북구   356768   358528  -1760\n",
            "11   동작구   306411   308937  -2526\n",
            "15   성동구   656857   661320  -4463\n",
            "24   중랑구   478850   484639  -5789\n",
            "1    강동구   844581   851717  -7136\n",
            "18   양천구  1095806  1103111  -7305\n",
            "21   은평구   404271   415561 -11290\n",
            "10  동대문구   582291   599080 -16789\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 428
        },
        "id": "e5310a5b",
        "outputId": "00ae9e48-3c26-4a29-efd6-b2f474446b2b"
      },
      "source": [
        "import matplotlib.pyplot as plt\n",
        "import matplotlib.font_manager as fm\n",
        "\n",
        "# 나눔 폰트 설치\n",
        "!apt-get -qq install fonts-nanum\n",
        "\n",
        "# 폰트 설정\n",
        "fe = fm.FontEntry(\n",
        "    fname=r'/usr/share/fonts/truetype/nanum/NanumGothic.ttf',\n",
        "    name='NanumGothic')\n",
        "fm.fontManager.ttflist.insert(0, fe)\n",
        "plt.rcParams.update({'font.size': 12, 'font.family': 'NanumGothic'})\n",
        "plt.rcParams['axes.unicode_minus'] = False\n",
        "\n",
        "# 설정 확인을 위한 테스트 출력\n",
        "gu_balance = df.groupby('자치구').agg(\n",
        "    총대여=('대여건수', 'sum'),\n",
        "    총반납=('반납건수', 'sum')\n",
        ").reset_index()\n",
        "\n",
        "gu_balance['차이'] = gu_balance['총대여'] - gu_balance['총반납']\n",
        "\n",
        "# 시각화 테스트\n",
        "gu_balance.sort_values('차이', ascending=False).plot(kind='bar', x='자치구', y='차이', figsize=(12, 5))\n",
        "plt.title('자치구별 대여/반납 차이')\n",
        "plt.show()"
      ],
      "execution_count": 17,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 1200x500 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAA/cAAAIKCAYAAABm/b+8AAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAPYQAAD2EBqD+naQAAbYZJREFUeJzt3Xd8FPW+//H3bnogIZRAQu+iFAFpYgEBD0pTETkickREBdQLF1REUYFzVNBzxAMoHEBBBDkqgo2iwpEmIFWlHhXphCItAdLz/f3BL3uzZJMsmt2Zgdfz8diHZnZ25s3sd2bnM+U7LmOMEQAAAAAAcCy31QEAAAAAAMAfQ3EPAAAAAIDDUdwDAAAAAOBwFPcAAAAAADgcxT0AAAAAAA5HcQ8AAAAAgMNR3AMAAAAA4HAU9wAASHrrrbf0yy+/FMu0Nm/erNmzZxfLtJzm7bffLpblOHv2bG3evLkYEl0eUlNTNWHCBB07dqxYprd8+XJ99tlnxTItAIA9UNwDACDpscce0+rVq4tlWp999plGjhzp872cnBydPXvWr1d6enqB8/jqq6/017/+1ed7e/fu1aBBg3T27Fmv4XXr1i32gm716tXaunWr5+9hw4YVy3IcOXJkwIvPM2fOaP78+crIyPD5/smTJ2WM8WtaU6dO1e23316c8bwcPXpUgwcP1o4dO4plejNnztTrr79eLNMCANgDxT0A4LJjjNGhQ4d08ODBfK/Dhw8XWjQX5Nlnn5XL5fJ6hYeHX/J0JkyYoJiYGL9eUVFR6tOnj8/prFmzRu+++67P9/bu3avJkyfr9OnTXsN//vlnnTx58pIzF2bkyJH617/+5ff4GRkZ2rdvn06dOlWsOSQpKSlJiYmJ2rJli8/3q1evrvnz53v+3rJli+6++24dPnw437j79+9X2bJltWrVKr/mffjwYe3cufOSM/fr1y9fu6pZs+YlTyevEydOaP/+/crKyvpD0wEAOAvFPQDgsnPw4EFVrlxZVapUyfeqVKmSypYtq6SkpEua5lNPPaWdO3fme12qAQMG6MCBA369RowYoS+++OKS51EcvvrqK8XFxeUrPF0ul2666aZLnt758+c1aNAgxcXFqXr16ipTpoyuuuoqvf/++787Y3Z2tipXrqzhw4dLktLT03XkyBGdO3fO5/gHDx5UcnKyX9POycnx+m+gvPrqq/na1PLly3/XtJYuXaqGDRuqXLlyqlatmsqWLatnn31WmZmZxRsaAGBLoVYHAACguFWpUkVnz571eUn1iy++qEmTJqlkyZKXNM3SpUurdOnSfzhbZGSkKleu7Ne4lStXlsvl+t3zatSokdzu33ccf926dQoPD9f27dvzTSMmJuaSpmWMUffu3bV161bNnz9fN998s44cOaJnn31WvXv31m+//ab/+Z//ueSMy5Yt06FDh3TPPfdc8mftoly5cipXrtwfns7KlSt1++2366GHHtKiRYtUpkwZLV68WI8++qh2796tDz74oBjSAgDsjOIeAHBZKlGihM/hq1atUteuXf0uUA8ePKgqVar4fM/lcqlq1apau3atEhMTf3fWgpw6darQAwq7d+8utPgfP368ypYt6/m7a9eufs87JydH0dHRuuaaa/z+TEE++eQTffnll/ruu+/UokULSVLNmjX1/vvva+/evRo8eLAGDx58ydOdM2eO6tatq2bNmnkNX758uY4cOZJvfH/vn/+90tPTtW7dOklSdHS0GjVqVOC4mzZtypc7l9vt1jXXXKMNGzYoMjLSr3k/8cQTuvXWWzVlyhTPsB49eig6OlqdO3dWv3791LFjx0v41wAAnIbiHgBwxdi0aZM2bNig0aNH+/2ZihUrateuXT4Lw/Hjx2vmzJl+F2CX6ujRo4We5a9ataq+/PLLfMPXr1+vBx54QO3bt/f7KoFAmjNnjho1auQp7HO53W499thj+u677zRlyhS1adPG81779u0LnWZqaqoWLFigJ598Mt977777rs+DO4G+xP7o0aPq0KGDJCk+Pl579uwpcNwmTZoU2K6ef/55LVq0SCEhIX7Nd9u2bfrxxx81atSofO916tRJtWrV0owZMyjuAeAyR3EPALhivPDCC2ratKluu+02vz/jdrt11VVX5Rt+8uRJzZ8/Xw899FCxXK7vy65du3zOO1dYWJjq1auXb3juWes+ffooIiIiINkuxfbt2wtc5p07d5Z0oVjP+28JCwsrdJqfffaZUlJSdN999+V7b8aMGbrxxhvzDQ8N9b3bU79+fU9/Ahs2bPC5TP1RtWpV7d27169xC2pXx48f11dffaW+ffsWuQxybd++XZLUrl07n+936tRJn376qV/TAgA4F8U9AOCKMH/+fC1atEjLly8v8FL29PR0z+PjoqOjC71fffjw4crJydHzzz8fkLyStHXrVnXp0sXney6XS+np6crJycmX8/z585IuFM5xcXGe4b7O8heHzMzMfI/dy+vQoUMF3rZQpkwZRUdH68yZM5c0zzlz5qhFixaqXbv2JX3Ol3//+99KTEyUy+VSrVq1JMlz1vzEiROexxJmZGQoPT1dBw4c0K+//qrdu3fryJEjl/S0gKKMHDlSaWlpGjZsmM/3U1NTPcs6t9+IQ4cOKSoqSqVKlfL5mSpVqvh8IgAA4PJCcQ8AuOzt3btXjzzyiKKiogq8f1660JP9gAEDJEmTJk3SY4895nO8qVOnavr06Z6iMBD27dunpKSkAu/Lrlu3rg4ePFjgpdulS5fW448/7nXLwMMPPxyQrFOnTtXUqVN/9+eNMYqOjvZ7/BMnTmjJkiV67bXXvIbnHrTxdfm9MabAe+4bNmyo6tWrew1LTExU7dq11aNHj3zjR0VFKTExUTVq1FCTJk2K7ZFzX3zxhaZPn64XX3yxwMfhderUyWv83CsfChPo2xEAAPZAcQ8AuKydOHFCnTt3Vs2aNRUTE6M777xTq1evVmxsbL5xX3/9dU/P6/Hx8T6nt2DBAg0aNEgxMTGaN2+eevTo4de90VdddZV++umnS87funVrz/9369bNc3l1r1691KZNG5+PdnO73apUqVK+vgCaN2+uhISES85QlL59++qvf/2rpAuXuF+sUqVKOnr0qM/P/vbbb0pNTVWdOnX8nt9HH32knJwc3XvvvV7D4+LiFBoaqnvvvVdlypTxei/3Kofy5cv7NY/Q0FBt375dv/76q7KysuR2uxUTE6O4uDifnTGGh4crPDzc73/DxX788Uf16tVLOTk5hfY38OGHH+r666+XJM93WalSJaWmpiolJcVntgMHDhR6UAsAcHmguAcAXLaOHz+uW2+9VZmZmfr8888VGhqqG264Qd27d9cXX3yRr/gtXbp0gR3QGWP0yiuv6Pnnn9cLL7yg3r176+abb1bPnj01Z86cIjvVW7p0aYHPX5eku+66Sy1atNCIESMKHOfie/srVqyoihUrFjrfvNavX+/3uJeiRIkSnuXm65aHa6+9VkuXLvX52c8//1zShX+/v+bMmaP27durQoUKXsNLlSql77//Xrt37853ttrlcqlcuXJeB0uKEh4eruzsbJ0/fz5fZ4AXu/feez1F96VavXq17r77bjVu3FjNmjVTly5dtHDhQp9Z4+Pj87XRa6+9VtKFNnbxcjTG6IsvvtDNN9/8u7IBAJyD4h4AcFnasWOHunTpoujoaK1YscJTCH711Vdq27atunTponnz5nndk16Qn376SYMHD9aKFSv07rvv6v7775d04bF6HTt2VKtWrTRz5kw1bty4wGkUdeY0IiJCpUuXvuTO3FJSUlSqVCm/HvNWoUIFffPNN7r66quLHDc0NFSpqak6deqU3G63srKylJGRoX379umnn37Szz//7Pfj6x544AF16tRJa9eu9SqAs7Oz9eabb+qee+7RmDFjvD5T0Nnrffv26dtvv9WMGTN8vl+/fn2fVw/8Xq+99pr27t2r5cuXFzree++9pxkzZvjdoZ50ofCeMmWKhgwZok6dOmn27NkqUaKEQkJC1KFDB02ZMkV/+ctfipxOvXr11KJFC/3rX//KV9x/+umn2rdvn/r37+93LgCAMxXcUxAAAA41Z84ctWzZUvXq1dOaNWu87ouvWrWqVq1apePHj3seW1aY559/Xg0aNNDZs2f1/fffewp7SapVq5Y2bNigatWqqXXr1pfcKVxxiImJ0a5du7Rz585CX1u2bNHRo0e1ceNGv6bbunVrnTx5UmXKlFFcXJzKlSunihUr6tZbb9W4ceP0888/KzU11a9p3X777brrrrvUvXt3LVq0SKmpqdq9e7d69uyppKQkjR8/XvXq1fN6FdRT/Pvvv6/IyEh179690Hned999+vbbb/3KVxz8ObiS19q1a9WyZUsNHTpUL730kubPn+95fN/f//53vfrqq+rfv7+6dOni1z3zEydO1PLly9W/f38dOHBA586d0wcffKC+ffvq4Ycf5sw9AFwBOHMPALis5OTkaOzYsXruuec0fPhwn5eJV6pUSWvWrPHrHvhKlSpp7ty5uvvuu32+X7p0aX366afatWtXgb2VB1rdunWLHCctLU2S/52rtWvXTidOnNDJkycVERGhiIgIRUVFKSoq6ndlnDt3rkaMGKGePXvq3LlzcrvduvXWW7V8+XJVqlTJ7+nMmTNHXbp08Xlv+cXz+9Of/qQbbrihwHHCwsLkcrmKfOTc+fPntWvXrkLH+e233wp9/2KPPPKI6tatqzlz5vjsb+Dxxx/XDTfcoE8//bTQpzbkatGihZYtW6YnnnhCVatWlXShD4Jhw4bpueeeu6RsAABnorgHAFxW3G63fvzxxwIfd5erRIkSatKkSZHTy+09vyi/99noxeGuu+7SF198UWSv7QU9W70gsbGxPjse/D0iIiL0+uuva9y4cUpKSlJ8fPwlHyj44YcftH37dr300kvFkumGG27w62DHhg0b/LqVoVq1an7Pe9OmTUV2wNekSRO/2miuG264QZs3b9bJkyd19uxZVaxYUaGh7OoBwJWCLT4A4LJTVGF/OcnKytKnn36q559/Xr169SpwPJfLpfj4+Hy9yAdbWFiY58zypZozZ45Kly6t22+/vZhTFa5NmzZF3nM/atQozZw50+9p/pGe9YtSpkwZy79nAEDwUdwDAOBgWVlZMsaoRo0all49EGjGGM2dO1c9evTwqzAOCQnRnj17irycXrpwwKFWrVo+33O73Tp9+rR27txZ6EGjgwcP+nX5PAAAgUJxDwCALhR4Rd177a/f88zz3/uc9Nz7xn/++We/CtmYmJhLuse9KBfnDgsLK5az0hdPZ8WKFTp48KB69+7t1+c7d+6sl156KV8v/L6Ehobq/PnzPr//1q1ba+7cubrmmmuKnIY/Pdv/Xv72D+Cv39veAAD25TKX2r0rAACwlbvuukuff/65srOzixz3+uuv15o1a4KQqnitWrVKq1at0ogRI66o2y4AAPAXxT0AAAAAAA7HzWEAAAAAADgcxT0AAAAAAA5HcQ8AAAAAgMPRW/4lyMnJ0eHDhxUTE0NnPgAAAACAgDPGKCUlRRUrViz0sasU95fg8OHDqlKlitUxAAAAAABXmAMHDqhy5coFvk9xfwliYmIkXViosbGxFqcBAAAAAFzukpOTVaVKFU89WhCK+0uQeyl+bGwsxT0AAAAAIGiKujWcDvUAAAAAAHA4insAAAAAAByO4h4AAAAAAIejuAcAAAAAwOEo7gEAAAAAcDh6ywcAAAAA+GSMUXZ2trKysqyOctkJDQ1VSEhIkb3g+z29YpkKAAAAAOCyYYzR6dOndfz4cWVnZ1sd57IVEhKi8uXLq1SpUn+4yKe4BwAAAAB4OXLkiE6fPq3Y2FjFxsYqNDS02M4w48LBk6ysLCUnJyspKUmpqalKTEz8Q9OkuAcAAAAAeGRnZ+vMmTOKj49XuXLlrI5zWYuJiVFERIR+++03lS9fXiEhIb97WnSoBwAAAADwyMzMlDFGJUqUsDrKFaFEiRIyxigzM/MPTYfiHgAAAACQD5fhB0dxLWeKewAAAAAAHI7iHgAAAAAAh6O4BwAAAABccY4ePSqXy+X1evnll73GmTt3rtq3b5/vs+np6RoyZIjKly+v2NhYde/eXYcPH/YaZ//+/YqIiAjaowTpLR8AAAAA4Lfqzyy0OoL2ju38h6dRoUIFnT17VsYYz7CoqCivcdLT05Wenp7vs3379lVSUpK+/fZbxcXFafTo0Wrbtq22bNni6YgwIyNDGRkZXtMPJIr7ACjOxl4cjRYAAAAAkN/veSLA999/r08++UR79+5VhQoVJEkTJ05UixYt9NZbb+mpp54q7ph+obi/QnDAAQAAAAAumDFjhvr165dveEREhGbMmKFevXoV+NlPPvlEbdq08RT20oUe7++77z69//77lhX33HMPAAAAALiiPPjgg8rMzPR6nTp1SmFhYSpdunShn921a5datWqVb3ibNm20Y8eOQEUuEsU9AAAAAOCKExoa6vV67bXXVLNmTXXs2LHQzx05ckRly5bNN7xixYo6f/68Tp06FajIheKyfAAAAADAFW3Tpk1644039PXXX8vlcnm99+2333qGLVq0qMBp5HacF6wO9C7GmXsAAAAAwBVr37596tKli6Kjo1WtWrV877du3VqpqalKTU3V7bffroSEBP3222/5xjt06JCio6NVpkyZYMTOhzP3AAAAAIAr0oYNG9S9e3c98cQTCgkJ0S233KKvvvpK1atX94zjcrkUGRnp+fuaa67Rt99+m29ay5cvV8OGDYMR2yeKewAAAADAFSUjI0Pjx4/Xyy+/rDfeeEMPPvigJCk8PFwtW7bUlClTdNddd/n87F133aWxY8fq6NGjnh7zjTGaO3eu/vKXvwTt33AxinsAAAAAwBUjJydHjRs3VqVKlbR+/XpdddVVnvf+93//V1dddZVGjBhRYMd6DRs21N13360///nPeuedd1S6dGmNHDlSaWlpevTRR4P1z8iHe+4BAAAAAFcMt9utDz/8UF9//bVXYZ+rU6dO+uGHHxQdHV3gNKZPn67GjRurVatWqlatmo4ePaply5Z5Xb4fbJy5BwAAAAD4be/YzlZH+MMaNGjwhz4fERGhN954Q2+88UbxBCoGnLkHAAAAAMDhKO4BAAAAAPAhIiJCERERv+uz4eHhCg8Pl8vlKuZU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2r59u9fBiGrVqql9+/YBy+aLFTsUdv3dmz9/vrKysjx/u1wu1apVS02bNs2XY+XKlfriiy/UqVMntW3bNmCZoqKilJGR4de4Tz/9tF555RVLt+fShSvX8hZAwfgOL+V3T5L69u2rt956K+DLyVebuuqqq9SoUSOv375Vq1bphx9+8NoedO7cWbVq1Sr2THm3UTk5OXrkkUe8tlEul0t9+vTRunXr9PPPP3ttT+vWrasbb7yx2DO988476tevn9ewF154QWPGjLFs3fO1LZ8yZYrX73DNmjXVtm1brVq1Sp9//rm6d++uVq1aBSxTUTgI+X/82XbOmDFDP//8s8aOHSuXy6Xhw4frr3/9a0DyFMWKfWGK+wApUaKE7rnnHkVFRSk9PV0ffPCBzp07J+n/iurcjX0wV9qiNv7h4eG6//779c0332jv3r2enjKvuuoqtW7dOiCZFi5cqM6dO/t8r0+fPnrvvfc8f1+87AKlQoUKqlSpklavXq3Q0FCVL19eSUlJcrvdXjsN7dq1065duzRp0iS9+OKLGjFihO67776AZDpw4IDfO4OVK1dWRESE32c9f6/mzZtr8+bNnr9dLpduuOEGrVixosANWjB2UCMjI5WZmXlJnzHGKCIiQqmpqcWep3///pe0g5orMjJS06ZNK/Y8F+vevbsqV66sv//9756d9+rVq2vp0qWqXbu2pOBtp26//XalpaUVOZ7L5dK4cePUvHnzgLdzf3KFh4fr888/V5kyZXTttdd6FUGtWrUKyIE/uxUYdv3du/HGG5Wdna3169erRYsWSk9P1+7du3XmzBmvHJs3b9bNN9+sDh06aOnSpfrmm2/UvHnzgGRKSkry2p7nHvz45ZdfFBIS4jVumTJlFBMTE5R2fs899+ijjz7y+V5YWJgyMjI8B0CD8R0W9ruXk5Mjl8ulunXravv27QoPD1eZMmUUHR0d8OV00003KSsry9OmUlNTtX//fp08edJr3YuJiVGrVq0UGRnp+ezTTz+tm266qdgzFbWNCg0N1ccff6zy5cvruuuu89pGdezYUc8880yxZ7r4IGPeNmPVuufPb0y7du3UpUsX3XLLLerWrZsWLlyo//znP54r2QLBjgdC7HgQcv/+/crIyFDdunU9B6ly/z/X1q1b1b9/fy1ZskQRERHq2LGj3nnnHd12220ByWS3fWGK+wC5+DIft9td4E5NMHdyitqoRUZGatGiRYqIiFCzZs08xc6OHTs8l7cUt5CQkAL/7YX9MASS2+32XII/YMAAvfDCC1q5cqXXypiUlKTKlStr1KhRev755zV37lzNnj1bCxcuDGi2SxHojazb7damTZs8lzydPHlSrVq1UnZ2dqEbtEDveF28Q5i78d+1a1e+Hee8wsPDVaVKlYDlKszevXsVHh7udS93sERERKhEiRJq3ry5Fi1apK1bt6p79+769ddfPePY5RJKX6w+o5krWAcfc9mtwLDr717efDk5OTLGKCQkRDk5OV45HnzwQZUpU0b/+Mc/9Oyzz+rAgQNeB5cDrairLYLRzgv7Pb54m27lNmHatGnavn273njjjXztLpjbg9w2lZmZqcjIyHy/fXa75DsYv795+WozudtIO617krRv3z6FhYV5foMHDBigKlWq6LnnntO4ceO0Z88eTZkyJWDzt+OBELsehJS8t1W5/5+enq6IiAj9+c9/VuvWrTV48GBJ0vTp0/X555/r008/DUgWu+0L+74WFH/YxZf22uWZjIsXL/Y5PDMzU8YYhYeHKz09XVlZWVqzZo0kKS0tTdHR0QHLZIzR4sWLbdXzpsvl0uzZs1W/fn3t2bNHd955Z75xcpfPDTfcIElq3bq1HnvssWDGlCQdPXpUN954o1asWBH0wtDlcunaa6/1bLT8vec50OuDrwLd5XKpdu3alt2C8sEHH+jPf/5zge/PnDlTYWFheu6554KW6ZZbbtHSpUuVmZmp7777Ti1bttSLL76ozMxMde3aNWg5/JWWlqaMjAxL758rSjC39atWrZJ0Ycdi7dq1ngLjYufOndPChQsDXmDY9XcvV26evLff5P3dWbNmjd59911JF85g33XXXUHPl5tr1KhRGjhwoCpUqBDUDMYYn33a2O1517Vq1dLcuXOtjuFZJgX9rthhmX300Uf66aefPL8twczka152XPck6a233lKJEiX0wgsvSJKWLl2qTz75RJJ0xx13FHiFaXHxtQ/sa9jEiRP16KOPeg6ETJgwIWAHQvJ2ypjL5XKpevXqhe5LBbKNrVixQm3atFHNmjU9w2rWrKnt27frscce0/Lly7Vx40a99NJLnvc7duwY0L5K7LYvTHEPSRcu/YmJidGzzz4rybvBBeo+5LyGDRtmq+JeunCJ6bPPPqvHHntMM2bM8Aw3xmju3LlasmSJpP9biRMTE3XmzJl89yUWl7Vr1yoqKkqNGzf2Gv7II4+oQYMGlpzxLYoddmzs4r777iu0uK9Tp46WLl0axEQXLu3Lvay7Vq1amjVrlu666y6VLFlS8+fPD2oWfwwdOlTJycmaPXu2JfM/dOiQxo4dq1WrVik7O1s33nijRo0aFfQC7GJOKDDswNdvTN5l8+uvv+qqq66SJNWuXVsHDhzw9B8QKBMmTFCFChX05z//WceOHZPL5dLXX3+t6dOnW9IRqiSfT/Awxli2TRg/frxefvllud1uPffcc/qf//kf1a5d23MZ7q233lpgvyVWsWKdS0pKUnh4uMqWLesZlpGRoaefflojRowIep6iWLXuHT16VD///LNycnIUHh6uJk2aKCIiQjVq1PAcMJUuLM8aNWpIutBvSqA7n3bKgRCrD0K2a9dO2dnZXpfh//zzz0pOTtbevXslXfjuKlWq5Hk/ISFBx44dC1rGiwV7e2CvreEVxBjjORtuh6K2SpUqWrlypSXzdrlc2rFjh8/3QkJCLPmRzP1O+vbtqxdeeEEpKSle70+YMMGzoc/t6Tw0NFTGmIBdgnfbbbfp7NmzatasmcaNG6e2bdtqwIAB+uWXXzxXEdhNu3btPN9fs2bN9Oqrr1qWJSoqyqstbdu2TXfddZf+85//BOVS/NynUxQkIyNDTZo0CXiOwnTp0kW9evXSxx9/rDZt2ujYsWOqU6fO7+6gsDhNmzZNCxYs0Pr16y2Z/y+//KLWrVvr+uuv15AhQ+R2uzV//nzVr19f69ev9zqLYBd2K+rt9LuXe6Vabo7z588rOztbJUuWlCSVLFlSxhidPXs2IFeK5F4pM3z4cMXExGjy5MmaPXu23G63Bg0apLFjx3o6dA2m3M6nfHnuueeC3qbef/99TZ8+XZ999pkyMzP10EMPqXz58urZs6eOHz+unJwcffnll0HN5I+IiIigt/GmTZvqt99+U9euXTVq1Cg1atRII0eOVGJioh555JGgZimM1etekyZNlJOTo7CwMCUnJ6t379566623VKNGDb3//vuejOnp6Z4rocLDw/2+9zwQOAj5f4wxPvu2MsboyJEjkqSYmBilpKR4tqHnzp0L6BXIRQn2vjDFvYU2bNgg6UJv0E2bNrU0S82aNS07G1YYY4wGDRrkWSmCdZ9Y7vwiIyPVr18/zZ071/P8TJfLpbVr12rGjBnq37+/Tp8+LUk6c+aMwsLCFBMTE5BMKSkp2r17t9544w3dfvvtqlevntLT0/XNN9+oVKlSAZnnH9WzZ0/PD061atUszXL27FnP/69YsUI9e/bU0KFDg3qP/YIFCwp8b//+/RozZkzQshTk1Vdf1aeffqply5apQ4cOWrVqlYwxysrKUosWLYKeJy0tTSNHjtQHH3ygr776yrI+EUaMGKG77rpL//rXvzzD/vKXv2jw4MEaNGiQ50oeO7GiwCiKnX73tm3bpszMTDVq1Mhz8C/30ZRpaWlyuVyKiIgIyLxbt26t5ORkpaen69ixY3rqqad03XXXqU6dOmrdurXuv//+gMz3jzDGqESJEl5/B9qkSZP01ltv6frrr5d04dLp559/Xvfee6/i4+N19OhRn5cOW+2HH36Qy+VS/fr1gzbPo0ePavHixZo4caKaN2+u22+/XRs3btR3330XtAz+snLdO3LkiDIyMhQaGqpZs2bp448/lnRhHyW3OHS5XIqLi9OpU6dUrlw5nTlzRnFxcQHJUxirD4TY9SBkQR0bLl68WKmpqapWrZq2bt3qeUrN9u3bLd0HDfq+sEFAuN1uk56e7vV3rqysrEL/tsLOnTtNjRo1jDHGpKWleeW5+O/iVti0XS6XGTZsmHnyySfNk08+aYYNGxaUZeV2u012drYxxphNmzaZxMREY4z3sli3bp1xuVxmzpw5xhhj1q5daxo0aBDQTLltaufOnebqq682rVu3NufOnSvwM8H47nKX08Xzc7lcXu8FK1NhTp8+bYYNG2bKlStn5s+fH9R5F/VvPn78uKedBYvL5TLp6en5vqthw4aZjh07eo0brO3UI488YoYMGWL++c9/mhEjRpiKFSuajh07msOHDxf4mWC0qdKlS5sff/wx3/ADBw6Y0NBQk5mZadm2PHeeeefvcrnMjh07zM6dO/P9HgUyh51/91wuV75seXNUrVrV/PDDD8YYY3bs2GHKlSsXsCxut9ucPXvWa90bP368cblc5rvvvvP5mWC086J+j3/44Qezbds2s23bNvP9998HPE9sbKw5f/685++0tDQTGxtrjDGmUaNGpnLlyqZGjRqmRo0aZtasWZ5xgtW2cueTmZnpte7l5OR43g/GunfxvBYvXmzKlCljevfu7TVOsH9/L95HyLu+Wbnu5WaaPXu26dq1qzHGmGPHjplKlSp5xmvevLn56quvjDHGLFu2zDRv3jxgmS7OZcyF5eNyucx///tfs23bNuN2u01OTo5xu90mJSXFGGPM+fPnjdvtNmlpaQHLlLudSk5ONo8++qgpX768ueGGG8xf/vIXn5+xsmaoUaOGSUpKMmPHjjW33367Z3iPHj3M3/72t4BmstO+MGfuIenC/eUnT560OkY+LpdLr776queIV1ZWlsaPHx/UDE2bNlV0dLS2b9/ueSyYJLVo0ULly5fXrFmzdN999+ndd98NWidk9erV0/r16/XnP/9ZHTt21JIlSzxnVEaMGKEVK1ZIunCpt13P6gfSgw8+qIiICMXExKh69epKSEjQ8uXLNXfuXLVr104//PCD7fooKFmyZL7bPwIt9/FRF19q+8QTT6hOnTras2eP557DYDDGKD4+XidOnNDHH3+s7777TsYY3XzzzSpdurTXuFu3btWpU6ckXbgqI9CXC6elpfk8Q1GiRAllZ2crKyvLc4uOVcxFZ1Lr1atnu0vzrVTUsrjpppu0ZMkSNWrUSF9++WXQn2s9ZMgQnTt3Tvfff782btyo2NhYffXVV5o0aZKkC20w75nzQDDGaO3atQX2T9CgQQOv3rwDLSYmRqdPn/aseykpKZ4zlLGxserTp4/ndqbcs+SX+hjU4nBx2zIWd0B42223ad26derQoYPeeOMNDRkyxLIs/iwHq9c96UJ7yvso3O7du2v06NEqX768Ro8erXvuuSfomXIf9Zi7rrlcLlWuXFm//vqrGjVqpL1796pMmTIBu8ohrxIlSmjKlCmqV6+ehg4dqtdffz3g87xUoaGhOnfunB599FG9/fbbaty4scLCwnTq1ClNnTrV6nhBQ3EfIBf/MIaEhGj69OkKDw/X+fPnC30clxXCw8M9hUXuhjj33vHc5xQHivn/jynzNfxiwfqxvPhe/7Zt22r16tWqXbu2J5fL5dKbb76pe+65R1WqVFFkZGRQL38rWbKkPv74Y/3pT3/S3Xff7bksOPeSfelCB1sNGjQIWAaXy6VPPvnEc4nY6dOnfX5H58+f93TcFowCtnr16jp37pxOnTqlzz77TFu3btWRI0dUo0YN3X333UpISAh4hosZYwq97D47O9uv57sXp6uvvlpS/nWtWrVqateunRYuXKjHH388aHlc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zKrtQU5OjgkJCTERERGmXLlyplmzZqZLly6mTJkypmbNmmbhwoUBm3dhevToYaZMmeL5e9q0aaZ06dLmnnvuMXfffbdl286Lt1OTJk0yd911l+cVGhpqzp496/WZQO9Q2K2dXw7FfUE7h8HKZHVx78+/P1jL6FIzBCtXTk6OCQsLM2632/Pfi9c7Y4z55JNPTMWKFc2mTZvMtm3bTI0aNTy/zYFQWHFvxe+ME5bTpk2bPAeP865f69atMy6Xy8yZM8cYc+HgfIMGDQKWyRj7bc+NuXDV4eTJk/MN//nnn01ERIQ5cuSIadSokVmzZk3QMt1xxx1m2rRpnr/ff/99061bN2OMtcW9nfaFeb5YEMTHx+umm27SN998k++99u3ba/r06ZIuPJ6kY8eOAc2Se59cyZIl9cILL+jTTz/VE088oQULFnjGadeunddzmadNm6Zbb701oLn8ERERoXPnzkmSzp8/r9DQ0IDcR+4EZcqUUa1atTyvkJAQ5eTkSJJ69OihkSNH6vPPP9eYMWMC+jxyqfB7M6UL90z7esZ0INm1nScmJurdd9/VU0895bOzoGBuD1wul3bv3q3Nmzfrk08+0dChQ9WwYUNdffXV2r9/vyZOnKj169cHbP4FueOOO/Tqq68qJSVFZ8+e1bhx49S1a1dde+21atSokWe8YG87L1anTh21bNlSLVu2VIsWLeR2u5WTk6MNGzZozJgxGjNmjF588UWFhYUFLINd27lTFLbdsoOL72unP4D/Y+WycLlcysjI0KlTp7Ry5Uo9++yzGjlypB588EGdOnXKM94dd9yhe++9Vy1btlSTJk10xx136O677w5YLmOMTp06pYyMDGVkZBTY+Vuwtp12XU55NW3aVNHR0dq+fbvX8BYtWqh8+fKeTuHeffddT2/+gWLH7fnWrVvVoUOHfMNr166tatWqadu2beratavX4znffvtt3XHHHQHL1KlTJ40bN07JyclKSUnR3/72tyIf95y7fxxIttoXDsohhCvQxZc5jR071gwZMsQY431k6dChQ6Z8+fKmSpUqpnz58mbfvn0BzeXrbMG8efNMXFycOXjwoCdTfHy86dKli+nRo4cpV66cOXDgQNAyFXTmvn79+p57kb7//vuAn2W1m8LOfJUrV84cPXrUGHPhqoZHHnnEVKpUydPmrMpljDFHjx418fHxAc+R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U88++6w++uijgMyb4h624XK5CiwMP/jgAz355JOecYYOHaq5c+cGOSH84etioNWrV1uQBAAAALDehg0b1KZNG0nSTTfdpPXr1wdkPjznHrZhjNH8+fN1/vx57du3z+u93bt3q3nz5p6/r7vuOv3yyy/Bjgg/DBo0yOuS/HHjxun111/X5s2bValSJQuTAQAAAIFljNHcuXOVlZUl6cKl+idOnFCVKlUkSZUqVdLZs2eVlpamyMjIYp03xT1sYfLkyXK73Xr++eeVk5Ojn3/+WR06dPC8f/r0aZUqVcrzd0xMjM6ePWtFVBRh0qRJkqTU1FQ99thjWrlypf7zn/9Q2AMAAOCKMHHiRM/VrBkZGZKksLAwSVJoaKiMMcrIyCj24p7L8mG5efPmafz48dq6dat27NihXbt2af369dq5c6feeOMNSVJCQoKOHj3q+czx48dVrlw5ixIjl69L8LOysvTuu++qXr16OnfunDZs2KD69etbkA4AAAAILpfLpTVr1mjVqlWSLjwlKjw8XKdPn5Z04aRlaGhoQJ78RXEPy40dO1b/+Mc/dPXVV3uGXXfddXrttdc0depUSVKjRo20bNkyz/vffPONrr322qBnxf8xxigsLEyRkZGKj49X8+bN1bVrV1WoUEFjxozR5MmT9cEHH6h06dJWRwUAAACCKm9fYg0bNtTWrVslSTt37lT16tUD0gk1l+XDctu3b1ezZs3yDW/VqpV+/fVXSVK/fv00aNAgNWnSRBERERo5cqRee+21YEdFHi6XS7t379a5c+d06tQp7d+/X1u3btWpU6f03XffaeLEiSpXrpxatGhhdVQAAACg2BljdOrUKc9Z+DNnzvgcr3379po+fbratWunt99+Wx07dgxIHop7WC4hIUG7d+9WYmKi1/CffvpJCQkJkqQ77rhDK1euVMuWLeVyufTYY4/p7rvvtiIu8qhWrZrn/2+44Qb16tVLkpSUlKRJkyapffv26tOnj8aPH6+IiAirYgIAAADFrmbNmipfvrznb2OMateunW+8//mf/1GTJk1UtWpVpaena8OGDQHJ4zK+bpoFgmjkyJFaunSpFi5cqLJly0qSjhw5ottvv13333+/hg0b5hk3JSVFkgJyjwqK3/79+3X//fcrNTVVS5Ys8Xy/AAAAgNNlZGTo6NGjysnJkSS53W5VqFBB4eHhys7O9vxXunDya/ny5br55psD1tE0xT0sl5GRoe7du2v16tWex92tX79e99xzj6ZNmxaQ+1EQPNnZ2XrwwQcVGRnp6UMBAAAAuJxlZ2crLCzMU/gHA8U9bGPVqlVav369JOnGG29Uy5YtLU6E4pSRkaHw8HCrYwAAAABBMW7cOA0fPjxo86O4BwAAAADA4XgUHgAAAAAADkdxDwAAAACAw1HcAwAAAADgcBT3AAAAAAA4HMU9AADIJysrK9/r4j54586dq/bt2+f77IcffiiXy+X1WrNmjdc4+/fvV2RkZL7PbtiwId9nfb1atGhRvP9gAAAcjuIeAAB4mTx5ssLCwvK9IiIitGDBAs946enpSk9Pz/f5Hj16KCUlxevVunVrr3EyMjJ8frZ58+bKyspSZmZmga+DBw9qw4YNSklJKf5/PAAADkVxDwAAvAwcOFDGGK/X+fPnFRISori4uCI/73a7VbJkSa/XpQgJCVFoaGiBr6ioKM98AADABfwqAgCAIn366aeqUKGC2rZtW+h4DzzwgM/L6MuWLatffvmlWLKcOHFC0dHRKlGiRLFMDwCAywHFPQAAKNKbb76pgQMHyuVyFTrezJkz811Gv2zZMqWlpalcuXLFkmXfvn2qWbNmsUwLAIDLRajVAQAAgL3NnDlT+/fv1+DBg4sc1+VyKTT0/3YvjDF6/vnn9fDDD/t1Sb8/duzYoYYNGxbLtAAAuFxw5h4AABTo4MGDGjZsmOrWreuzd/tvv/3Wc+n94sWL873/z3/+UwcOHNDo0aOLLdP69evVoEGDYpseAACXA4p7AADg0+nTp9WlSxfde++9OnjwoF5++eV847Ru3VqpqalKTU3V7bff7vXep59+qieffFJVqlTxeWAgr3Xr1vn1CDyXy6U5c+boueee8/z973//u1j/3QAAOBHFPQAAyOfYsWPq0KGD6tevr4kTJ+qzzz7ThAkTNHPmTK/xXC6XIiMj8xXvU6ZMUf/+/fXll18qMTFR3bp107lz5wqcX6tWrQp89N3QoUP1yCOPFPj+vffeG4hFAACAo1DcAwAALz/++KNatWql5s2ba9asWXK73apTp46++uorPfPMMxo7dmyBnz106JB69eqll156ScuWLVP79u3173//WxUqVFDLli21cePGAj9b0KPvcs/QF/Q+AACguAcAAHkcOXJEbdu21VNPPaXJkycrJCTE816jRo20du1ahYeH+/zsrl271KBBA1WsWFE7duxQo0aNJF0o2mfNmqWHH35Yr7/+elD+HQAAXGk43A0AADwSEhK0c+dOVahQwef7NWrU0NChQ32+d9VVV2njxo2qVauWz/f96W0fAAD8Ppy5BwAAXgoq7IvicrkKLOwBAEBgUdwDAAAAAOBwFPcAAOB3iYiIUERExO/6bHh4uN+fDQsLo+M8AACK4DLGGKtDAAAAAACA348z9wAAAAAAOBzFPQAAAAAADkdxDwAAAACAw1HcAwAAAADgcBT3AAAAAAA4HMU9AAAAAAAOR3EPAAAAAIDDUdwDAAAAAOBwFPcAAAAAADjc/wM5ZgVdeWwxXgAAAABJRU5ErkJggg==\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "000d8b75"
      },
      "source": [
        "# 런타임 재시작이 필요한 경우 아래 코드를 실행하세요.\n",
        "# 실행 후에는 첫 번째 셀부터 다시 실행해야 합니다.\n",
        "import os\n",
        "os.kill(os.getpid(), 9)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "df_t = pd.read_csv(PATH + '따릉이_고장신고 내역.csv', encoding='euc-kr')\n",
        "# 한글 깨지면 encoding='euc-kr' 또는 'utf-8' 시도\n",
        "\n",
        "print(f'데이터 크기: {df_t.shape[0]:,}행 × {df_t.shape[1]}열')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "G0m0J4zg_hAk",
        "outputId": "eb87e239-e068-44fd-c8df-50ace265dd2b"
      },
      "execution_count": 18,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "데이터 크기: 59,305행 × 3열\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "7adc86ba"
      },
      "source": [
        "### 가설 검증: 고장 신고와 대여 건수의 관계 분석\n",
        "**가설**: 고장 신고가 빈번한 대여소는 서비스 품질 저하로 인해 대여 건수가 적을 것이다."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 224
        },
        "id": "6a0060fe",
        "outputId": "84966faa-971a-4acc-8113-6484d9ddb5fb"
      },
      "source": [
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "import seaborn as sns\n",
        "\n",
        "# 1. 고장 신고 데이터(df_t)에서 대여소별 고장 건수 집계가 필요한데,\n",
        "# df_t에 '대여소명' 또는 '대여소번호'가 있는지 확인이 필요합니다.\n",
        "# 만약 df_t에 대여소 정보가 없다면 분석이 어려우므로, df_t의 컬럼을 다시 확인합니다.\n",
        "print(\"df_t 컬럼 목록:\", df_t.columns.tolist())\n",
        "display(df_t.head())"
      ],
      "execution_count": 22,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "df_t 컬럼 목록: ['자전거번호', '등록일시', '구분']\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "       자전거번호                 등록일시    구분\n",
              "0  SPB-72175  2025-07-01 00:08:14   기타 \n",
              "1  SPB-52313  2025-07-01 00:27:42   기타 \n",
              "2  SPB-68508  2025-07-01 00:34:17    체인\n",
              "3  SPB-46541  2025-07-01 00:43:50  타이어 \n",
              "4  SPB-46541  2025-07-01 00:43:50    안장"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-5eb99507-e86c-4c56-aa68-59f1dcbeaf97\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>자전거번호</th>\n",
              "      <th>등록일시</th>\n",
              "      <th>구분</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>SPB-72175</td>\n",
              "      <td>2025-07-01 00:08:14</td>\n",
              "      <td>기타</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>SPB-52313</td>\n",
              "      <td>2025-07-01 00:27:42</td>\n",
              "      <td>기타</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>SPB-68508</td>\n",
              "      <td>2025-07-01 00:34:17</td>\n",
              "      <td>체인</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>SPB-46541</td>\n",
              "      <td>2025-07-01 00:43:50</td>\n",
              "      <td>타이어</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>SPB-46541</td>\n",
              "      <td>2025-07-01 00:43:50</td>\n",
              "      <td>안장</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-5eb99507-e86c-4c56-aa68-59f1dcbeaf97')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-5eb99507-e86c-4c56-aa68-59f1dcbeaf97 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-5eb99507-e86c-4c56-aa68-59f1dcbeaf97');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "summary": "{\n  \"name\": \"display(df_t\",\n  \"rows\": 5,\n  \"fields\": [\n    {\n      \"column\": \"\\uc790\\uc804\\uac70\\ubc88\\ud638\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 4,\n        \"samples\": [\n          \"SPB-52313\",\n          \"SPB-46541\",\n          \"SPB-72175\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub4f1\\ub85d\\uc77c\\uc2dc\",\n      \"properties\": {\n        \"dtype\": \"object\",\n        \"num_unique_values\": 4,\n        \"samples\": [\n          \"2025-07-01 00:27:42\",\n          \"2025-07-01 00:43:50\",\n          \"2025-07-01 00:08:14\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\uad6c\\ubd84\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 4,\n        \"samples\": [\n          \"\\uccb4\\uc778\",\n          \"\\uc548\\uc7a5\",\n          \"\\uae30\\ud0c0 \"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "02610bd8"
      },
      "source": [
        "일반적으로 따릉이 고장 데이터에는 대여소 정보가 포함되지 않는 경우가 많습니다. 만약 `df_t`에 대여소 정보가 있다면 아래 코드를 실행하여 분석을 진행할 수 있습니다. (여기서는 `df_t`에 '대여소번호'나 '대여소명'이 있다고 가정하고 예시 코드를 작성합니다.)"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "f371b5e8",
        "outputId": "39bac831-282d-4372-def9-f877cb39ef86"
      },
      "source": [
        "# 2. 대여소별 고장 건수 집계 (예시: df_t에 '대여소' 관련 컬럼이 있다고 가정)\n",
        "# 실제 데이터에 맞춰 '대여소명' 부분을 수정해야 할 수 있습니다.\n",
        "if '대여소명' in df_t.columns:\n",
        "    fault_counts = df_t.groupby('대여소명').size().reset_index(name='고장건수')\n",
        "\n",
        "    # 3. 데이터 병합 (df와 fault_counts)\n",
        "    merged_df = pd.merge(df, fault_counts, on='대여소명', how='left')\n",
        "    merged_df['고장건수'] = merged_df['고장건수'].fillna(0)\n",
        "\n",
        "    # 4. 시각화 및 상관계수\n",
        "    correlation = merged_df[['대여건수', '고장건수']].corr().iloc[0, 1]\n",
        "    print(f\"대여건수와 고장건수 간의 상관계수: {correlation:.4f}\")\n",
        "\n",
        "    plt.figure(figsize=(10, 6))\n",
        "    sns.scatterplot(data=merged_df, x='고장건수', y='대여건수', alpha=0.5)\n",
        "    plt.title(f'고장건수 대비 대여건수 분포 (상관계수: {correlation:.2f})')\n",
        "    plt.xlabel('고장 신고 건수')\n",
        "    plt.ylabel('대여 건수')\n",
        "    plt.grid(True)\n",
        "    plt.show()\n",
        "else:\n",
        "    print(\"df_t 데이터에 대여소 정보가 없어 직접적인 Join이 불가능합니다. 자전거 번호별 분석 등 다른 접근이 필요할 수 있습니다.\")"
      ],
      "execution_count": 23,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "df_t 데이터에 대여소 정보가 없어 직접적인 Join이 불가능합니다. 자전거 번호별 분석 등 다른 접근이 필요할 수 있습니다.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "df.columns"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "M3V-ZxO5Ev0N",
        "outputId": "9c47884e-2f78-4171-d735-46b1643d92b8"
      },
      "execution_count": 21,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Index(['자치구', '대여소명', '기준년월', '대여건수', '반납건수', '위도', '경도', '거치대수'], dtype='object')"
            ]
          },
          "metadata": {},
          "execution_count": 21
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "fe2be655"
      },
      "source": [
        "### 가설 검증: 대여소명에 '역'이 포함된 곳은 대여/반납 차이가 클 것이다\n",
        "**가설**: 지하철역 인근 대여소('역' 포함)는 출퇴근 등 특정 시간대 쏠림 현상으로 인해 대여와 반납의 불균형(차이)이 더 크게 나타날 것이다."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 848
        },
        "id": "8d09442d",
        "outputId": "34b0ac8c-98f6-48e2-b242-2bba67856037"
      },
      "source": [
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "import seaborn as sns\n",
        "\n",
        "# 1. 대여/반납 차이 계산 (이미 df에 '차이'가 없다면 계산)\n",
        "df['대여반납차이'] = (df['대여건수'] - df['반납건수']).abs()\n",
        "\n",
        "# 2. '역' 포함 여부 컬럼 생성\n",
        "df['역세권여부'] = df['대여소명'].str.contains('역').map({True: '역세권', False: '비역세권'})\n",
        "\n",
        "# 3. 그룹별 평균 차이 계산\n",
        "station_analysis = df.groupby('역세권여부')['대여반납차이'].mean().reset_index()\n",
        "display(station_analysis)\n",
        "\n",
        "# 4. 시각화\n",
        "plt.figure(figsize=(8, 6))\n",
        "sns.barplot(data=station_analysis, x='역세권여부', y='대여반납차이', palette='Set2')\n",
        "plt.title('역세권 vs 비역세권 대여/반납 차이 평균 비교')\n",
        "plt.ylabel('대여/반납 차이의 평균 (절댓값)')\n",
        "plt.grid(axis='y', linestyle='--', alpha=0.7)\n",
        "plt.show()\n",
        "\n",
        "# T-test 등을 통한 통계적 유의성 확인 (선택 사항)\n",
        "from scipy import stats\n",
        "station_group = df[df['역세권여부'] == '역세권']['대여반납차이']\n",
        "non_station_group = df[df['역세권여부'] == '비역세권']['대여반납차이']\n",
        "t_stat, p_val = stats.ttest_ind(station_group, non_station_group)\n",
        "\n",
        "print(f\"역세권 대여반납차이 평균: {station_group.mean():.2f}\")\n",
        "print(f\"비역세권 대여반납차이 평균: {non_station_group.mean():.2f}\")\n",
        "print(f\"P-value: {p_val:.4f}\")\n",
        "\n",
        "if p_val < 0.05:\n",
        "    print(\"결론: 통계적으로 유의미한 차이가 있습니다. 가설이 지지됩니다.\")\n",
        "else:\n",
        "    print(\"결론: 통계적으로 유의미한 차이가 없습니다. 가설을 기각합니다.\")"
      ],
      "execution_count": 24,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "  역세권여부      대여반납차이\n",
              "0  비역세권   95.894174\n",
              "1   역세권  119.928760"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-fafb7ecd-2ee9-4251-88e2-f6cc0395b54f\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>역세권여부</th>\n",
              "      <th>대여반납차이</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>비역세권</td>\n",
              "      <td>95.894174</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>역세권</td>\n",
              "      <td>119.928760</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-fafb7ecd-2ee9-4251-88e2-f6cc0395b54f')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-fafb7ecd-2ee9-4251-88e2-f6cc0395b54f button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-fafb7ecd-2ee9-4251-88e2-f6cc0395b54f');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "  <div id=\"id_992cba22-ff6b-429c-8dfb-6120e0d6544d\">\n",
              "    <style>\n",
              "      .colab-df-generate {\n",
              "        background-color: #E8F0FE;\n",
              "        border: none;\n",
              "        border-radius: 50%;\n",
              "        cursor: pointer;\n",
              "        display: none;\n",
              "        fill: #1967D2;\n",
              "        height: 32px;\n",
              "        padding: 0 0 0 0;\n",
              "        width: 32px;\n",
              "      }\n",
              "\n",
              "      .colab-df-generate:hover {\n",
              "        background-color: #E2EBFA;\n",
              "        box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "        fill: #174EA6;\n",
              "      }\n",
              "\n",
              "      [theme=dark] .colab-df-generate {\n",
              "        background-color: #3B4455;\n",
              "        fill: #D2E3FC;\n",
              "      }\n",
              "\n",
              "      [theme=dark] .colab-df-generate:hover {\n",
              "        background-color: #434B5C;\n",
              "        box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "        filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "        fill: #FFFFFF;\n",
              "      }\n",
              "    </style>\n",
              "    <button class=\"colab-df-generate\" onclick=\"generateWithVariable('station_analysis')\"\n",
              "            title=\"Generate code using this dataframe.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M7,19H8.4L18.45,9,17,7.55,7,17.6ZM5,21V16.75L18.45,3.32a2,2,0,0,1,2.83,0l1.4,1.43a1.91,1.91,0,0,1,.58,1.4,1.91,1.91,0,0,1-.58,1.4L9.25,21ZM18.45,9,17,7.55Zm-12,3A5.31,5.31,0,0,0,4.9,8.1,5.31,5.31,0,0,0,1,6.5,5.31,5.31,0,0,0,4.9,4.9,5.31,5.31,0,0,0,6.5,1,5.31,5.31,0,0,0,8.1,4.9,5.31,5.31,0,0,0,12,6.5,5.46,5.46,0,0,0,6.5,12Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "    <script>\n",
              "      (() => {\n",
              "      const buttonEl =\n",
              "        document.querySelector('#id_992cba22-ff6b-429c-8dfb-6120e0d6544d button.colab-df-generate');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      buttonEl.onclick = () => {\n",
              "        google.colab.notebook.generateWithVariable('station_analysis');\n",
              "      }\n",
              "      })();\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "station_analysis",
              "summary": "{\n  \"name\": \"station_analysis\",\n  \"rows\": 2,\n  \"fields\": [\n    {\n      \"column\": \"\\uc5ed\\uc138\\uad8c\\uc5ec\\ubd80\",\n      \"properties\": {\n        \"dtype\": \"string\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"\\uc5ed\\uc138\\uad8c\",\n          \"\\ube44\\uc5ed\\uc138\\uad8c\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"\\ub300\\uc5ec\\ubc18\\ub0a9\\ucc28\\uc774\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 16.99501832485684,\n        \"min\": 95.89417448666728,\n        \"max\": 119.9287598944591,\n        \"num_unique_values\": 2,\n        \"samples\": [\n          119.9287598944591,\n          95.89417448666728\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {}
        },
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "/tmp/ipykernel_13972/1767294409.py:17: FutureWarning: \n",
            "\n",
            "Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `x` variable to `hue` and set `legend=False` for the same effect.\n",
            "\n",
            "  sns.barplot(data=station_analysis, x='역세권여부', y='대여반납차이', palette='Set2')\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 800x600 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAr4AAAIpCAYAAABE5DuOAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAPYQAAD2EBqD+naQAAbF1JREFUeJzt3Xd8VFX+//H3nZk0EpIgEELvVZosCIIKUkRQWLEgiAq6uKJ+EcWKlaKCqLgrdlDQXVApolgAQbHCikAUKZEWDCQBQgstdeb+/uCXSyYzCTPJhATm9Xw88niQzz1z5nMy5OYzZ8491zBN0xQAAABwnrOVdwIAAADA2UDhCwAAgKBA4QsAAICgQOELAACAoEDhCwAAgKBA4QsAAICgQOELAACAoEDhC1QQ8+fP1/r168s7jXK3ePFifffdd+WdhofVq1dr8eLF5Z2GTzZt2qT//ve/pe5n+/bteuONNwKQ0fnju+++C9j/g8zMTL366qvav39/QPo71wXyZwsUhcIX8MI0TR07dqzYNvPmzdMFF1xQ4ufYv3+/PvnkE+v75557LiAn/W+++UZr1qwpdT/lZdq0aZo9e/YZ2x0/flwul6vI46mpqYqOjtYvv/xSojxcLpfmzZunI0eOSJI+/vhjTZs2rUR9FbRp06aA/3E/cuSI5s2bJ6fTKUlavny5nnzyyVL3+9NPP+nee+8tdT9nsnr1av3+++9ej2VmZurEiRM+99W2bVt9+eWXgUrNw+zZswPy/0CS9u3bpzFjxmjz5s0B6c+bkSNH6plnnimz/gMpkD9boCgUvkABW7Zs0Q033KCoqChFR0crOjpat9xyi3bu3OnR9uTJkzp8+HCx/W3fvl2jRo3S0aNHPY6tWrVK119//RlzSkhI0Nq1a71+/fnnnx7tJ02aVGFm6ebNm6fQ0NAij/fp00f33HOPz/0dOXJEY8aMUVxcnCpXrqzw8HD17t1b3377rUfbnJwcHTt2TJmZmcX2effdd3udaT969Khuuukm/fbbb8U+fvv27UW+Pn/88YdycnLc2s+fP1/33XffmQfrh99++0033XTTGf8/FnTw4EElJycrNzc3oLlI0kMPPaRhw4Z5Pfbaa6+pa9eubrFx48bplVde8dr+3nvv1YABA3x+7j/++EPp6em+Jyvpr7/+kmEYHl8ffPCBX/0UlJOTo7/++st641RaP/zwg8LDw8/4FRERoeTkZOtx27dv119//eW1z7ffftvruL19VapUSdOnT/c53zVr1sgwDKWkpHg9fuedd6p///4+93f48GElJib69LV9+/Yy+X+N84OjvBMAKoo1a9aoZ8+eatGihWbMmKEGDRpo27Zteumll9S2bVt999136tixo1997ty5U2+//baeeuopRUdH+51TZmamevfubc3kFZaRkaEXX3xRDz30kN99nw05OTnF/gHKzc31KAyLcujQIXXt2lUZGRl66qmndNFFF2n//v2aPXu2evXqpTfeeEN333233/m99dZbuvTSS9WhQwe/HpvvnnvuKXKG/fjx47rqqqv0xRdflKhvSbryyiv17bffev0/sGLFCvXq1cuv/lasWKEHHnhAGzdulCSFhYXpxhtv1LRp01S9evUS5fj2229r9OjRSk9PV0xMjA4cOKB9+/Z5bXvkyBGlpqb63LfL5Sp2Zj8Q6tatq8TERJmm6RavV6+e332dPHlSDz30kGbPnm296ercubOmT5+uTp06lTjHTp06WUuAhg0bpm7dullvGv/66y8NGTJEs2bNUsuWLVW7dm2f+rz55pvVvXt3n9qOGjVKy5cv1+jRo31qn/97XdTvvz+/+5L04osvavLkyT63v/DCC63/40BBFL7A/3fnnXeqZcuWWr16tRyOU78aXbt21dChQ9W5c+cS/dHKzs6WJIWEhJQop4iICB08eLDI47169dIff/xRor7PNRMmTFBaWpo2btyounXrWvHrrrtO99xzj/Xlj9K+PpL09ddfF3ls0qRJmjlzZon7lk4Vqk8//bSGDBnicaxhw4Z+9fXDDz+oX79++sc//qGvvvpKF1xwgZYsWaK77rpLq1ev1q+//qoqVar4neP777+vK6+8UjExMX4/tiKw2Wxq3rx5qfsxTVPXXXed/vjjD33yySe6/PLLtXfvXj3++OPq3r27fvrppxK/wYqIiFCXLl2sf9eqVcv6PjY2VpLUunVrNW7cWCEhIW5FfIMGDbz2WblyZbVo0cKn569SpUqxn96Uteeff17PP/+8T20XLFigG2+8UceOHVPlypXLODOcayh8AZ2amd2wYYMWL15sFb35QkND9dxzz+nqq6/Wl19+qUaNGkmSFi1apMcff7zYfg8dOiRJZXbydTqd5frH6GxatGiRRo4c6Vb05ps8ebJmzJihJ554wioQU1JS1Lt372L7PBdeH9M01aBBA58LlOKMHj1affr00VtvvWXFbrjhBlWqVElXX311idas79y5U6tXr9bcuXPd4vv379eCBQs82m/atMn/xP20Y8cO/e9//5MkNWvWrNhxDRgwoMgZ+aioKL3yyisaOXKkT8/76aefatmyZfrll1908cUXS5IaNWqkuXPnqmvXrrr33nu1evVqP0fjnypVqigxMdGaJb/tttsC0u+BAwf0t7/9LSB9lbX8N28sd4A3FL6ApL1790oqemakSZMmktxnSGrWrOlTvzExMYqIiAhMooWkpKT4/FHluW7v3r1Fvj4xMTGqXr267Ha79fqEh4f71Kfk22tZEikpKapVq1aZ9O2vjRs3asOGDRo/frzHsf79+6tx48aKiIjQ/Pnzrbgvb+7mzp2rqKgo/f3vf3eLJyUl6dlnn/Vov2/fPoWFhZVsED568cUXrTXDr7zyiu68884i27777rvWG6CC9u3bpx49ehS5ZMObOXPmqG3btlbRm89ms+nee+/VbbfdpsTExIC8iSlOs2bNrH9XqlQpIH2mpqb6dE1CRVDU0jBA4uI2QJIUHx8vSUVeiJF/sUh+O19t2bJFLperTE7EOTk52rVrl9sfOX88/PDDRT52/fr1stvt1kVfa9as0VVXXaULLrhAERERatSokZ566imfn6uoC2a+//57n/uIj48v8vU5ceKEDh48WKLXRyq7maE///yzxK9PoOXPtPbs2dPr8f79+2v37t1q0aKF9eXLG4I5c+bo73//u0eB1blzZ/32228eX0XtEjFnzhxFRUUpKipKw4cP93N07t566y0dP35cx48fL7bolaS4uDi3Med/LV26VCEhIRoxYoTPz7tp06Yif75XX321JJX5jK8kbd261brQ6+TJk6Xuz+l0avfu3WratKnfj23YsKHX3/3333+/1HkVJX8XkEAV/Ti/MOML6NTHkRdddJGmTp2qq666yu2Y0+m0ZsmuuOIKK+7LH5S1a9fq2LFjWrdunS6++GKNGzdOU6ZMCUjOf/zxh/Ly8hQaGqrPP/9cmzdvPuMf+YIGDhyol156SevXr/dYd/jRRx+pfv366tChg7Zv367evXurX79+WrhwoWJiYrR3715lZWX5/Fz5BWZh/nwMe8MNN+jdd9/VQw89pGrVqrkdmzRpknJycvT4449rwoQJknyb9Vm7dq0kaeXKlerUqZOWLVvm8fqXlMvl0u+//65u3bppxYoVSkxMtNZklpUTJ04oPDzc60VDKSkpioiIKHIdbt26db3uPlKc9evXKzExMSBbUF111VXWtltxcXFW3G6369ixYzpx4oR1QVROTo6OHDminTt3aufOndqxY4f69OmjgQMHljoPSdq1a5deeeUV3XbbbV4vFHM6nTp+/LikU0uh8pezpKSkFPlm4YILLlClSpWKfPMWKIcPH1aLFi3c1vi2atWqVH1u3rxZubm5uuiii/x+7IoVK7z+DMeNG3fGLSNL6sSJE7Lb7T596oPgQ+EL/H8zZ85U9+7ddfXVV2vcuHFq2LCh/vzzT02cOFFr167V1KlT3S78WbVqlWbNmlVkf8nJydq0aZPCwsL07rvv6uKLL9bDDz9szWZ98803+r//+78iHz9nzhwtXrxYeXl5ys3NVXZ2tjIzM5WRkaEjR45YWzbdcccdaty4sZo1a6ahQ4f6PN5LL71UtWvX1qJFizwK3wULFlhrZT/99FOZpqm5c+fKbrf73H9BRX2068+MzNNPP61ly5bpsssu05QpU9ShQwft27dPM2bM0IwZM3Tvvfeqffv2VvuDBw/qscceK7bPJUuWKCwsTLNmzdLDDz+s7t27W0X6sWPHPD6yLighIUFTpkyR0+m0CrKsrCwdPXpUR44c0cGDB3X06FFNmzZNn3zyiZo2bRqQC6iKU3ApSP369f16rMvl8ntJzpw5c1S9enX16dPHLW4YRpE7MRQVr1q1qtddUy699FK99957ioqK8niOatWqqXbt2mrZsqUiIyP9yr0oOTk5uvXWWxUdHa0XXnjBa5uffvrJWhfesWNH/frrrz71XXjXiEBwOp3WG51du3bp5MmTmj17tvWmskePHm7tU1JSVLdu3RLlUrCAnTZtmh544IEzPqZx48ZelyjFxMSUuPB9/PHHddVVV+nyyy+XdOr3eN26ddbe1QcOHPB4cwzko/AF/r8OHTpozZo1GjdunHr37q3s7GyFhoaqd+/eWrVqlUdx6HA4ii1833vvPVWqVEkvv/yyHnjgAY0fP141a9a0LrRJTEwsNp9q1aqpUaNGcjgcCg8PV6VKlfTbb7/pgw8+0IoVK1SzZk3VrFmzRFfhS6cKhxtuuEGffPKJJk2aZMXXrl2rpKQk3XzzzZJOrX/NycnR1q1b1bJlyxI9VyDExsbq559/1lNPPaU77rhDhw4dkmEY6tChgz766CMNHjzYrf2uXbuKLXy/++477dy5U++++67++c9/6rPPPtO1115rFeln2n81KipKjRs3lmEY1v6pWVlZeuqpp/Tiiy+qd+/eqlmzpuLi4mQYhvW4n3/+ueQ/hDPYsGGDqlSponfffdfj/2bt2rWVmZlZ5JXu/n6U7XK5rJ974QtCq1evrjlz5ujCCy90G3v+8/jzBuD2229Xv379dODAAUmn1m7HxMSoSpUqHs8rndqhozQXFN5555366aef1Ldv3yJ/t7p06WKthS745q127dpFrgk+cOCAMjMzS7RFmiTNmDHDKjQzMzOVmJiol156yfpkwzAM3XnnnYqPj1fjxo11/fXXe30zUKtWLbeL3/J9/vnneuSRR7Rx40a3N7gXX3yx/u///s/j0xlft0wrC++8844uuOACq/D9+eef9dFHH1mFb3p6umrUqFFu+aFio/AFCmjZsqU+/fRT5eXl6ejRo4qNjZXN5n0pvMPh8PqHVzr1ceNrr72mESNGaOTIkXrttdc0duxYffjhhz7n0rdvX/Xt29ctNnv2bH3wwQfW3q05OTmaOnWq2x+x5OTkIi8CK2zw4MH697//ra1bt1prURcsWKA2bdqodevWkqSbbrpJn332mTp16qTRo0dr7NixJd7vtbRiY2M1ffp0vfrqqzpy5IgiIyOLLHLyX5uitiqbNGmSWrdurdtvv11r1qzR2LFj1bdvX59nPZs2beqxvdKuXbv01FNPqWPHjtbs8/vvv6+0tDSrzU8//eRT/yVRs2ZNVatWzetyhnbt2kk69dHzoEGD3I6ZpqkvvvjCupGDL1auXKnU1FSvN6oYP368evXq5fXmISEhIX7vDlC1alWtWbNGPXr0OON+2J9++mmJPpLPycnRXXfdpblz52ratGl68cUXdeutt+r999/3+D0PCwtTnTp1PPpo166dVqxY4bX/zz//XNKpGeySGDJkiDp16iSn0ym73S6bzaaQkBBFRkZaN9sp6lxVkGEYXted5y/7ad68udt4bTabtQ76XLF7926fz4EIPhS+gBcOh+OMWzvdcsst1qxoYQ888IBcLpfGjx8vh8Oh1157TT179tQNN9wQ0Cuj8/LytHbtWrfC15+PDy+55BLVrVtXCxcu1Lhx4ySdurNYwe2bHA6H5s2bpxUrVmjixIl69dVXNWrUKI0fP/6M24DlF1EnT570WNZgmqYyMzN9LrQK93umme46deooLy/P6/KM2bNn69tvv9Xy5ctlGIaeffZZffLJJ3rkkUf8ujuVLzZt2uR25z9/bt4gnVrjeuDAAR09elROp1N5eXk6cuSItm7dqq1bt6pGjRo+7RzRokULXXzxxXr77bc9Ct/PPvtMKSkp+uabb9z6Km5Xhzlz5qhhw4a65JJLPI5VqlTJ401baaSkpOjvf/+7Vq5c6fHRfWFXX321Zs2a5ddFaX/99Zduvvlmbdq0SZ999pn69++vq666SldeeaX69u2rDz/80G3dcVGGDx+u/v37a/Xq1W4/F6fTqddff109e/a0tkP0V+XKld2W8hR27NgxHThwQIZhKC4uzvp969+/f7m8US34u+/NyZMnS/S774s9e/YUu0wJwY1dHYBC7rnnHp9u4elwOLwWsdOnT9cHH3yg999/3/qD06NHD40dO1a33XabEhISApZrpUqVNG/ePC1YsMD6uvDCC31+fP5yh0WLFkk6dbFSUlKS17XCvXv31g8//KAFCxZo1qxZPl1I1KRJE9lsNkVGRnr8/Gw2m9asWeP3leLz5s3z6fWx2Wxq0aKFx218f/nlF91777168MEHrX1+q1WrppkzZ+r111/X22+/7Vc+ZzJ16lS316fwkowz6d69ux5++GHFxMToggsusGbf8pdn+HOb4unTp+u7777TyJEjtXv3bp04cUIff/yxRowYoSeeeMK6c+GZdnXIysrSwoULi3zjl+/TTz/VmDFj/Brv2XTy5Ek9/fTTatmypVwul3799VfrNrr5N7M5evSoWrZsqa+++uqM/fXr10+DBg3Sddddp6+++kqZmZnasWOHBg8erG3btum1114LaP7p6el65JFH1KRJE0VHR6tRo0Zq2LChKleurLZt2+r555/XPffco9tvv93r4x9++GGvN0YJhPr16yssLMxa7lL4a/78+SXaJcIX3377bcAuIsb5hxlfoJBJkybpvvvuO2O7uXPnenzUnZiYqPvvv18vvfSSBgwY4HZsypQp2rlzp3VRmj/eeecdpaenq3Pnzh4XEpXWTTfdpH/961/as2eP5s+fr0suuaTYjwn79eunN998U0OGDNHmzZuLvWK8c+fO2rt3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AFBWyuTObQUVtUQCAAAAOJt8nvF99dVXlZWVdcZ2d9xxh6pVq2Z9P3jwYN14440aPHhwyTIEAAAAAsDnwnfz5s3Kzs6WdGoW9z//+Y9uvfVWjz16jx49ahW+GRkZWrJkiV5++eUApgwAAAD4z+fC96233rL+nZeXpw8++EDvvfeebLaiV0u8/PLL6tWrl+rVq1e6LAEAAIBS8rnw/f33360ZX6fTKcMwtGbNGqvwDQ8PV9u2ba32v/76q1577TWtXbs2wCkDAAAA/vO58B0yZIjbGt969epp6NCh1vdRUVH6448/JEmrV6/W9ddfr7fffluNGjUKYLoAAABAyfhc+G7ZsuWMbT766CN9+eWXWrZsmd566y1dd911pUoOAAAACJRSb2f29ttv68knn5QkhYWFKTU1VcePH9e+fftKnRwAAAAQKD4XvlFRUcrLy/OIV6tWTevXr5ckDRo0SN98842++OILPfPMM5o6dWrgMgUAAABKweelDidPnpTL5fKIN23aVJs2bXKL9ezZU8uWLdPll1+ubt26qVu3bqXPFAAAACgFn2d8C+/Xmy8uLk779+/3iF900UWaOHGi7r333pJnBwAAAARIqdf4VqpUSTk5OV6P3XXXXdq1a5e+//770j4NAAAAUCp+Fb7e1vjm81b8VqpUSffdd5/S0tL8zwwAAAAIIJ/X+NavX1+VK1f2eqxZs2YKDQ31emzixIklywwAAAAIIJ8L382bNystLc3jAje73a46deoEPDEAAAAgkHwufMPDw9WwYcOyzAUAAAAoM6W+uA0AAAA4F/g847tmzRplZWUVebxjx446duyYtm3bJunUut+4uDiNHTtW06ZNK32mAAAAQCn4XPjefPPNys7OliSlpqaqVq1a1jHDMDR79mzdeuutcrlcMk1T4eHh2rVrl/79739T+AIAAKDc+bzUYfv27dq9e7d2794t0zS1Y8cO6/vk5GT17NlT+/fvV1pamlJTU60tzEzTDHjSW7Zs0U033aS4uDhFRUXpb3/7m95//32PdpmZmRozZozi4+MVFRWlnj17KiEhIeD5AAAAoOIr0RrfgndxO3nypEfcZrOVScErSVu3blWXLl0UExOj77//Xps3b9Ztt92mO++8U//617/c2g4bNkzr1q3TkiVLlJiYqMsuu0w9evRQcnJymeQGAACAisvnpQ4F5Re1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C+3bt0/Hjh3TunXrdO+993pcGRgdHa0333xT6enpyszM1E8//WTtQAEAAIDg4tNShyeeeEKbN2/W2LFjdeedd+rmm29W9+7d1blzZ7fbBJ88eVK//PKLvv/+e/33v/9V48aN9eqrr+rCCy8sswEAAAAAvvB5jW+rVq20dOlS/f7775o5c6buuecebdu2TZGRkYqJiVFGRoZOnDihJk2aqFevXpo/f74uuuiisswdAAAA8JnfN7Bo166dpk+fLunUXTLS0tJ05MgRxcbGqmbNmqpSpUrAkwQAAABKq0R3bstXpUoVCl0AAACcE0p85zYAAADgXELhCwAAgKBA4QsAAICgQOELAACAoEDhCwAAgKBQ6sLX5XLp7rvvDkQuAAAAQJkpdeGbm5urd955JxC5AAAAAGXG5318d+7cqU2bNiksLEzVqlVT7dq1VaNGjWIfk5eXp06dOikhIaHUiQIAAACl4XPhe8kll+iCCy6Q3W7XkSNHdODAAdlsNjVr1qzIxzidTm3YsCEgiQIAAACl4XPhm56eruTkZIWFhVmxffv2acOGDerbt69effVVK24Yhm666SbFxMQENlsAAACghHwufA3DkGEYbrEaNWro8ssvlyQtX77c7Vjv3r0pfAEAAFBh+Fz4FscwDH3++ece8ezs7EB0DwAAAJQa+/gCAAAgKFD4AgAAICgEpPA1TVMXXHCB9VW1alX973//C0TXAAAAQEAEZI2vJC1ZssTt+3bt2gWqawAAAKDUfC58TdMs8phhGOrcubMkaf369crLy9OGDRt07Ngxj50gAAAAgPLgc+Hbv39/ORzFNz958qSuvvpq5eXlSZJsNpsGDRpUugwBAACAAPC58P3iiy/O2KZSpUpKS0srVUIAAABAWSj1xW02m00hISGByAUAAAAoM6UufENCQpSVlRWIXAAAAIAywz6+AAAACAo+r/Fds2bNGWd227Ztq+joaC1dulSSdNVVV8lmo7YGAABA+fO58B06dKhycnKUmpqqWrVqSZLbvw3D0PPPP693331XmzdvlsPhUPPmzfX111+fcTcIAAAAoKz5PB27Y8cO7d69W6Zpavfu3R7/Tk5OVm5urg4dOqRdu3YpKSlJJ0+e1OzZs8swfQAAAMA3pV6HkJmZqbVr10qS5syZo8cff1wREREKDQ3VuHHjKHwBAABQIfhc+E6YMEGS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xbHb7Vq6dKkaNGigK664Qo0bN9b8+fP15Zdfqnv37la7hQsXqnXr1urTp4+aNGmiFStWaPny5dYVxQAAAAguPq/xHT9+vIYNG6Znn31WN9xwg8cFYnv37tX8+fP1zDPPaNasWQFPtKC4uDj95z//KbZNdHS03nzzTb355ptlmgsAAADODT4Xvtddd50iIiL0+OOP67777lP9+vWttb4HDhxQcnKy/va3v2nevHnq3bt3mSUMAAAAlIRf25n169dP/fr1019//aXExERre7D4+Hi1atVKdevWLZMkAQAAgNIq0Z3b6tevr/r16wc6FwAAAKDM+HxxW1GGDh2qEtz8DQAAADirSl34zps3T7m5uYHIBQAAACgzPi912LNnj3JycrweS0pKUkhIiNdjoaGh3CkNAAAA5c7nwrdp06bKycnxuqyhZcuWRT4uPDxcJ0+eLFl2AAAAQID4XPhmZmaWZR4AAABAmSr1Gt+CTp48qYULFwaySwAAACAgAlr4PvHEE3r33XcD2SUAAAAQECXax3fgwIGKiorSI488ovbt20uS/vOf/2jhwoX69ddfA5kfAAAAEBA+z/i2aNHC2tXhyy+/VGhoqDp37qz/+7//08yZM/Xoo49q2bJlqlGjRpklCwAAAJSUz4Xv1q1blZeXJ0kyTVPvvfeeEhISlJCQoLvuukvz5s0rdncHAAAAoDyVaI2vYRiSpFatWmnlypUaMmSIHnvsMWVnZwc0OQAAACBQSn1xW2hoqObMmaMaNWpo6NChgcgJAAAACDifC99+/fopNDS0yOMffPCBtm7dqo8++iggiQEAAACB5HPh++WXX8rhOLUJREhIiLXcIV9kZKRee+01Pfjggzp+/HhgswQAAABKqURLHY4ePepR+EpSjx49NHr0aB05cqS0eQEAAAABVaJ9fItb8vDYY4+VOBkAAACgrAT0zm0AAABARUXhCwAAgKBA4QsAAICgQOELAACAoFDiwvedd94JZB4AAABAmSpR4et0OnX33XcHOhcAAACgzLDUAQAAAEHB5318GzZsKKfTaX1vmqbq1atnfT98+HDNnj3b7cYWhmGoQ4cOWrRoUYDSBQAAAErG58L3o48+UlZWVpHHGzdurN69eysvL099+vTRypUrdfToUQ0aNCggiQIAAACl4XPh27lzZ4/YkiVLFBkZqcsvv1ySVKdOHTmd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cmQpR4lgRuELeJGXl+dxn/B8ubm5bseys7OVnZ1tfb9v3z797W9/U61atfTTTz9p9erVat26tS655BK34jc3N1fZ2dlF3lc8MzOzyNzyny8zM9OtmI2JiVFubq7H14kTJ7Rlyxa3IjkrK6vIW3cDCE6333675s2bp3fffVd//vmnnnzyST355JOaOnWqW7ucnByv54/izlv57QufMyVpwYIFXs9dAwcO1KpVq6x2nLdQWuftnduA8vL000/riiuu0L/+9S8rNmHCBGVlZalZs2Y+95OVlVXkrHNxvN2cxTAMmaYph4NfeQDerV69Wp999pm2bt2q+Ph4SVL9+vXVvHlztWvXTo8++qhb+xEjRnj0UdLzlmEYXs9Pdrud8xYCihlfIMB+/vlnXXPNNR7xQYMGKSIiQqZpyjRNj6UPhR06dKjIW2v7a9++fZKkqlWrBqQ/AOefn3/+WR07drSK3nytW7dW06ZN9fHHH1vnr27dunntI5DnLenUuatatWoB6w+g8AUCzDRNGYbh9Vj+zKsvAnnCT0pKUnx8vCIiIgLSH4DzT3HnLl8FulBNSkpSw4YNA9YfwOcHQDFK8kfg0ksv1RdffKFbb73VLT5//nyFhYXpgQcekHTqQrSiuFwuJSUlWWtyXS6XtZbX3623nU6n1qxZo+rVq2vOnDlKTEz0+hElgODWrVs3TZo0Sfv27VONGjWs+O+//65t27bpk08+sdbbJiUlee1jx44dbtcS5F/HUNQ1E8XZv3+/du7cqV27dunll19WrVq1/O4DKIwZX6AY+R/rFfzydsVyQZMmTdLKlSv10EMPadu2bUpKStLTTz+t119/XSNHjlRsbKxiY2NVuXLlIvv4448/lJOTo59//lnSqTXCERERioiIUHJystfHfP3112rfvr3atm2rFi1aqH79+rrgggsUERGhl156SeHh4fryyy/lcDhUqVKlkv9QAJyXunbtqkGDBumaa67Rjz/+qL179+rTTz/VwIEDddVVV6lFixbW+cvbtQSStHbtWm3atElHjhzRzp07FR4eroiICD333HNFPm+XLl3Uvn17tWrVSo0aNVKNGjUUFham5s2bq2XLllq8eLFSUlIUFxdXVkNHEGHGFwiwuLg4rVu3TmPGjFH79u3lcrnUqVMnLVu2TN27d7fabd++XdOnT/fax9dff63atWvrq6++0qFDhzRhwgRNmDBBktSgQQOvj+nSpYveeecdORwOhYWFqVKlSmrUqJG2bt2qpk2bBnycAM4/7777rl588UUNGTJEe/fuVYMGDTRq1Cg9/PDDbheZrVixwuOx+/bt04YNG1S7dm3NnTtX99xzj/UJ1fjx47V3716vz/nWW28pLy9PYWFhCg8P1+OPP65mzZp5LZZXrlwZoJEiWFH4AmWgbt26+uSTT4ptU3CvyoJcLpdmz56tsWPH6rvvvtO///1vq+gtTnR0tC6++GKvz5OvJB83AggeDodD48aN07hx44ptZxiGbDb3D41nz56tCy+8UOPHj9djjz2mkSNHKjQ09IzP2b59e7fvIyMj3c5bTqfTKqD9XeoFFMZSB8CL/JNu4b0m89fa+rL2N39dW1Ff9evX1549ezw+Mnz//fd16NAh3XXXXXr++ec1bdo0bd26tdRjevPNNxUSEmJ9paSklLpPAOen4s5deXl5WrhwoYYNG2a1P3DggKZOnapnnnlGgwYNUtWqVTVlypRS53Hs2DGFh4db563nn3++1H0iuFH4Al7ExcWpVq1aCg8Pd5uZtdvtysnJUaNGjc7Yx5AhQ9wKTW9f1157rdtjNm7cqPvvv1/vvPOOIiMj1bp1a40dO1aDBg3SsWPHfM7f5XJJOnU3pPydHO6++263tcq1a9f2/QcCIGgsWbLkjOeuRo0aacOGDZJO3Yxn2LBhuuKKK6w7sOUvmSh417UzyT83de7cWc2bN5ckVa5cWbm5udaxJ554okzGjOBB4Qt4ERERoZSUFK8Xtx04cEB169Y9Yx8F97z09rVp0yb9+uuvbrcsHjx4sB555BENGDDAij399NNq165dkRe1edOkSROtXr1aCxYsKLbALe3WRQDOP/369Sv23GWaplq1aqU//vhDkjRt2jQdOHBA7733ntVHq1at9Oabb2r9+vU+P++kSZM0atQo3X333R674hTEeQulwRpfoJzkr30ruGZt5cqVbtsISafuXDR37ly/+s7JyfFYplHYsmXL1KRJE7/6BQDp1Pkr/5OlMWPGaNSoUYqOjnZrc8stt/jVpy/nrfvuu8/tNu2Avyh8gQqkcNFbUjabTSdOnCj2nvaNGzfmVqAASi08PFzh4eGl7sdmsykrK6vY81Z0dLTHRXWAP/irB5RSWFiYwsLCztrzhYeHW88XHh7u9arpLl26aMCAAWe8Avq2227T+++/XyZ5AkC+gsVxUefMDh066MUXXzzjHSbtdrsyMzMVEhJSJrni/GaY7A0ClIt9+/Zp+PDh+uqrr5jBAHBOeeihh9S3b1/16dOnvFMB/ELhCwAAgKDANBMAAACCAoUvAAAAggKFLwAAAIIChS8AAACCAoUvAAAAggKFLwCUsby8PL3wwgtq0KCBwsPDddFFF3m9G1/Pnj313//+1yP+ww8/qH79+h7xZ599Vvfff7+kU3fPmjx5stvx66+/XoZheHxFRUXp66+/dutn5MiRHv3n5ubq4YcfVnx8vC644ALdeuutOnTokEe7xo0b68cff/SIN2/eXMuXL/f8gRTQt29fzZw5s9g2ABAoFL4AUMZuv/12zZs3T++++67+/PNPPfnkk3ryySc1depUt3Y5OTle71qVmZnptd+8vDyrfXZ2tsftXhcsWKDc3FyPr4EDB2rVqlVWu6LulvXPf/5TK1as0CeffKLVq1fLbrdr0KBB1q1q83l7bklyOp1yOp1F/FROt8nLyyu2DQAECnduA4AytHr1an322WfaunWr4uPjJUn169dX8+bN1a5dOz366KNu7UeMGOHRR1ZWVonuUmUYhtfbUtvt9jPernrLli3673//q61bt6phw4aSpFmzZqljx46aP3++brrpJp9y6Nev3xnb3HDDDT71BQClxYwvAJShn3/+WR07drSK3nytW7dW06ZN9fHHH8s0TZmmqW7dunnt49ChQ6pSpUrActq3b5+qVatWbJvPPvtMnTt3tope6VQhPXz4cA0ZMsRt6URKSkqR/SxZssQan7evXr16BWxcAHAmFL4AUIZM05RhGKXqw5dC1R9JSUluBa03iYmJXgvxK664QqGhocrJybGK19q1awcsNwAoSyx1AIAy1K1bN02aNEn79u1TjRo1rPjvv/+ubdu26ZNPPrHW2yYlJXntY8eOHUpNTbW+z87OlmmaJVobu3//fu3cuVO7du3Syy+/rFq1anltt3fvXl144YUe8Zo1ayonJ0epqaleL7gryDAMnTx5stg2mZmZpX5jAAC+ovAFgDLUtWtXDRo0SNdcc42mTZumpk2b6n//+5/GjBmjq666Si1atLDa2u12r32sXbtWmzZt0pEjR3To0CE1btzYOnbXXXd5fUyXLl2UlZVlXTB34sQJHTlyRJUqVVLLli21ePFiNW/eXO3bt9emTZv8Hpdpmmds0759e91www3Fto2MjFSrVq38fn4AKAkKXwAoY++++65efPFFDRkyRHv37lWDBg00atQoPfzww24Xma1YscLjsfv27dOGDRtUu3ZtzZ07V/fcc49VSI4fP1579+71+pxvvfWW8vLyFBYWpvDwcD3++ONq1qyZnnvuOY+2K1eu9IjFx8crPT3dI56WlqaQkJAiZ4oLmj9//hnbAMDZROELAGXM4XBo3LhxGjduXLHtDMOQzeZ+6cXs2bN14YUXavz48Xrsscc0cuRIhYaGnvE527dv7/Z9ZGSk25ICp9NpFdDeZmRbtWqlzz//3CP+zTffKDc3V2FhYUU+d8G+/WG321n2AKBMcXEbAJwleXl5xX4tXLhQw4YNs9ofOHBAU6dO1TPPPKNBgwapatWqmjJlSqnzOHbsmMLDwxUSEqKQkBA9//zzHm2uvfZa/fLLL27rjl0ul95//3395z//cduZofDFbZGRkVbf/nzdfffdpR4bABSHwhcAzoIlS5acsfBr1KiRNmzYIOnUXdOGDRumK664wroDW/6SiYJ3XTuT/OK0c+fOat68uSSpcuXKys3NtY498cQTHo9r1qyZhg8frmuvvVY///yzEhMTNXz4cIWHh2vIkCHFPmdWVpbXrcs+//xzNW/evMitzd566y0/fqIA4D8KXwA4C/r161fsframaapVq1b6448/JEnTpk3TgQMH9N5771l9tGrVSm+++abWr1/v8/NOmjRJo0aN0t13361bb721yHbelhi88cYbuvLKK3XdddfpkksukdPp1OLFi8948wsAqKg4ewFABREaGmrdDnjMmDEaNWqUoqOj3drccsstfvWZk5Pj9XbCBd13333KycnxiIeEhOjFF1/Uiy++6NdzAkBFReELABVQeHi4wsPDS92PzWZTVlaWsrKyimwTHR3tcVEdAJyPKHwB4BxVsDgOCwvzutNChw4d9OKLLyoiIqLYvux2uzIzMxUSEuJ3HkU9d2EOh4NlEgDKlWGWZM8ZAEDAPfTQQ+rbt6/69OlT3qkAwHmJwhcAAABBgUVdAAAACAoUvgAAAAgKFL4AAAAIChS+AAAACAoUvgAAAAgKFL4AAAAIChS+AAAACAoUvgAAAAgK/w+g3a5gY+KTSAAAAABJRU5ErkJggg==\n"
          },
          "metadata": {}
        },
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "역세권 대여반납차이 평균: 119.93\n",
            "비역세권 대여반납차이 평균: 95.89\n",
            "P-value: 0.0000\n",
            "결론: 통계적으로 유의미한 차이가 있습니다. 가설이 지지됩니다.\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import seaborn as sns\n",
        "import matplotlib.pyplot as plt\n",
        "\n",
        "plt.figure(figsize=(8, 6))\n",
        "sns.barplot(x='역세권여부', y='대여반납차이', data=df, palette='pastel')\n",
        "\n",
        "plt.title('역세권 vs 비역세권 대여/반납 쏠림 현상 비교', fontsize=16)\n",
        "plt.xlabel('대여소 위치', fontsize=14)\n",
        "plt.ylabel('대여/반납 차이 (건)', fontsize=14)\n",
        "\n",
        "# 차트를 'chart.png'라는 이름의 이미지 파일로 저장\n",
        "plt.savefig('chart.png', dpi=300, bbox_inches='tight')\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 684
        },
        "id": "27kSH0opEy5i",
        "outputId": "4d25626f-ab97-4f73-d9a0-86e4e90bd552"
      },
      "execution_count": 25,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "/tmp/ipykernel_13972/624251742.py:5: FutureWarning: \n",
            "\n",
            "Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `x` variable to `hue` and set `legend=False` for the same effect.\n",
            "\n",
            "  sns.barplot(x='역세권여부', y='대여반납차이', data=df, palette='pastel')\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 800x600 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAsAAAAIsCAYAAAD8qR8WAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAPYQAAD2EBqD+naQAAW4tJREFUeJzt3Xd0VNXexvFn0iaBkAQQCEF6j3SkgyBIkypKE6R7ARVpKqJIk6aIioigIAIi5UoTpYMognpVQAQEQ28hFCkJIWWSzPuHK/MyziSZJBOScL6ftWYt5pw9+/yGwMkze/bZx2S1Wq0CAAAADMIjuwsAAAAA7iUCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADmRAVFaW4uDjb88OHD+vatWvZWFHOd+vWLe3fv19JSUnZXYqD6Oho3blzx/Y8LCxM4eHh2VhR+ty4cUOJiYm25+6u/9SpUzp16pTb+svNrl+/rgsXLjjd99dff6W4zx3u3LmjGzduZFn/95OrV6/q4MGD2V0GciACMO47rgSrwYMHq2TJkmm2O3v2rP74448U99eoUUOjRo2yPa9Tp47mzp3rWqEuqFixovr27eu2/twpMTFRf//9d6ptbt68qfj4eLttK1euVO3atXX69GmXjuPKz3P69Ony9vaWxWJJtV1ERIR+/fXXFPd37NhRXbt2tT3v1KmTXn/9dZfqdEXr1q3VtGlTt/V3t6tXr6pAgQJavny5bZu76+/Vq1eG/z0ePnxYS5cuTXH/oUOH9Pnnn9tt2717t0wmk3744Qenrzlx4oSGDh2qs2fPulzHihUr5OHhoZMnT7r8GmcmTpyoevXqOd3XtWtXjR49Os0+Ll26pB07djh97Ny5UwcOHHD6uhdeeEFVqlTJUN1HjhzR+++/7/CYM2eOEhISHNq7+n9Lks6cOaNvvvnGaT/JNm/erEceeSTNc8e/xcfH6+bNm6m2uX79ut0HQEl69913VaNGDYftgFd2FwC4wxdffKH58+frt99+k8ViUYkSJfTEE09ozJgxKly4sEP7uLg4u5HblEyZMkXr1q1LcVQ3MTHR7sRqtVrl7OaKO3bs0MqVK1M8jre3t5o2baoePXpkqM7s8MUXX6hv3746deqUSpcu7bRNwYIFNW7cOE2aNMm2LfnvJ7WbUG7evFnvv/++fvzxR0VHRyskJETt2rXTmDFjVKZMGYf2cXFxSkhIUGJiory9vVPs99NPP9W4ceMUFRUlf39/h/2u/jwPHDiQ6gcdT09P1ahRQ0OHDnWoM70/z507d2rdunWKjY112GcymTR16lQVLlzYVrcr9Sfbtm2b5s6dq6NHj8rPz0+1a9fWK6+8okqVKjltn1Z/qVm5cqWmTp2qPn36ON2/atUqTZ06Vc8884xtW/KHp5T+zk6cOKH58+ere/fuLn2gTe7LarW6FOhS8+233yowMNBh+7Vr13Ts2DEVK1YszT6+/PJLDR8+PNU206ZN09ixY+22JSUlZTjQ/fTTT5oyZYrDdk9PT/Xt21cBAQF22139vyX98zN89dVXFR4erqJFizptc/bsWf3www+KiopSwYIFXa57xowZmjhxoiwWizw9PR32nz9/XiVKlNCiRYvUv39/2/a0zjfDhw/XBx984HIdJpNJZcuW1fbt21WqVCmXX4echwCMXM1qtapPnz5atmyZmjVrpvHjxysoKEhHjhzRJ598oiVLlmj79u2qWbNmhvqPi4tL86Tvips3b+rMmTMp7j916pQWLFigNm3aKCgoKNPHuxeSR2ZT+0WckV/Ur732mqZPn65atWrplVdeUZEiRXT8+HEtWbJEy5Yt05o1a9SmTZsM1ZwcpDL7M42Ojk715xkeHq5PPvlEjRs3VtWqVTN1rOeee07Xr19X7dq1HfaZTKYMB7kpU6bojTfeUGhoqNq3b6+4uDitWbNGy5cv19q1a/X4449nqu4nnnhCe/bs0dWrVzPVT060YcMGHTlyRJK0ceNGtWvXzrbv3Xfflclk0pYtW/T999+nOuL/3HPPpfiBICEhQaGhoTp69Khbax80aJAGDRrk1j6TJU/LyIpzWFJSUqofwJx9AHTFhAkTHD6opubcuXNq3bq1du7cqYEDB6brWMhZCMDI1RYsWKBly5ZpxowZGjNmjN2+cePGqX79+mrZsqXD14XHjh1zqf+YmBj5+Phkus6nnnpKTz31VIr7ly5dqr59++rKlSu5JgBnhW3btmn69Ol67rnn9OGHH8pkMtn2jR8/Xs2bN9dTTz2lhx9+2O51qYXRu8XExEhSpn+mjRs31o4dO1Lcv3v3bjVt2lSXLl3KdAC+deuW2rVrp8WLF2eqn7t99913Gj9+vHr16qUlS5bYRtSmTp2qVq1aqUePHgoLC1NwcHCG+r927Zq++eYb/ec//3FbzTnF4cOH9eyzz2rgwIHy9/dX//79tWPHDlWrVk1btmzR22+/rfnz52vHjh3q2rWr9u7dq/Llyzvty8vLK9X/7z4+Pm6ZK//3339r/fr1Lo/eBwUFpXq+SsmRI0dUokQJ+fn5pfu12aVAgQIqUKCAy+2TR/bvvlYAuRMBGLnaxx9/rOrVq+uVV15x2BccHKz58+erbdu28vb2VvXq1W37rl275tLFasnzKrNa8iieO8J2bvbxxx+rcOHCtlG0u+XLl09Lly5VaGioYmJi1KRJE9u++Ph4l+aBXr16VUFBQQ59u1tO/3nOmDFD+fLl0/z58+2+Tg4KCtKyZctUuXJlNWzY0GH60JEjR+z+H6Vk5cqVSkhIUO/evR32HT582Olr7vVI8fHjx21zVUNCQlz6f75t2zZ1795dDRo0sH1AO3bsmBo2bKh+/fppwYIFevnllzVo0CD16NFDrVu3Vv369bV06VK7UWJXWSwWt/xbDQsL04gRI1wOwCVKlEh3AI6NjdV3330n6Z8Pbc6mh9wPkn8eOfEiXqQPARi52tGjR9WnT58Uf0kkf/3YsGFDu3mo165d05YtW9Ls/9KlS07nnLrblStXJElFihTJ8mPlZEePHlXNmjVlNpud7q9cubIKFy6sSpUq6Z133rFtnzhxon766ac0+7906VKGRzXTIyf/PKOjo/Xtt9+qe/fuTudBV6hQQc2bN9d3332np59+2m6fqysbLFu2TGXKlFGDBg0c9mV2RNxdOnbsaPvzyJEj9e6776bYNiIiQi+88ILWrl2rF198UW+//bbtw838+fNVo0YNLVy4ULNnz9aQIUMkSf7+/tq2bZuef/55tW/fXi1bttSCBQvs5ir/+uuv6t27t8LDwx3CaWJiomJjY1OcX58eDRo0UFRUVKb7Sc0XX3yh27dvS5IWLVqkkSNHptr+5MmTtnntRYsWzTWBOblmd0yNQ/YiACNX8/HxsX2t7Ux0dLQkycMj/QuexMfH69SpUwoJCclwfa46deqUihQpkqmvDidOnKi33npLp0+fdhryNm/erHbt2mnHjh1q3ry5oqOjNXPmTK1YsULnzp2Tt7e3SpYsqf/85z8aNmyYy8cdMGCA8uTJk+G675bWzzMpKUkxMTEZ+nlK/wRsZxfQuNupU6dkMply5EUyx44dk8Vi0SOPPJJim/r162vHjh167rnn7P79pzbtI9mJEyf0v//9T2+88YbT/SmtbDB37lwtXLjQ6b5PPvnE9oG1bt266tatW5p1pGXDhg22cJnWBWvJo3179+51CPV58+ZVu3bt1KNHDz322GMO+xYvXqxnnnlGn3/+uQoVKmS3/+uvv1ZYWJjmzZsnX19fu30mk0kFChRQq1atMvT+UnsvS5Ys0Ycffqi//vpLPj4+qlevnsaPH297b2vXrtW3334rSfrll1/S7PP27dt68803VblyZVWsWFETJ05Up06dUh08uPvvasKECZo4caJL9bdr187pgEdq5w13Sp764K5zHrIPARi5WqNGjbR169YUv3JbtGiRpH+Cz91XPqe2tFmyQ4cOKSEhQT///LNiY2Pl6+urxYsX252o3bXW59GjR1W5cuVM9dGpUydNmjRJ69atc3pRx+eff66CBQvagk/37t21e/dujRw5UjVq1JDVatXZs2fTfWIvWbJkiqM3W7duTVdfjRo10qJFi3T27FmnV/WvXr1aUVFRunTpkt3Pc/fu3Wn2fe3aNZ0/f16SbP3v2LHD7oKgiIgINWvWLF01O3P06FGVLFkyW+ZCXrlyRSdOnJAkpxfIRURESEp9dPrBBx+UJF28eDHdHwCXLVsmSU6nP0j/LB3oTGr1/Prrr/rzzz8l/fMhKb0B+Pbt2woPD1eFChVs28qXL5/iahf/FhISotWrVzvdV6hQIU2fPl0lS5bUxx9/7HTec4sWLdSiRQuH7cmrXCSPGme1hIQEPf7447b5yX369NGdO3f01VdfqWHDhrYVJyIiImzXSbgyVWzIkCE6f/68du7cqUqVKqlatWp68skntX37dj3wwANOX3P69OkMfUAsV66c0wAcFRXl0nkgNTExMUpMTLT7ZiQ6Olomk8l2XkweSf/3ahnIfQjAyNXGjx+vxo0bq1evXvrss8/sRli++OILjRs3TkWKFNHhw4ft5h5evHgxxa/ZkyWHt9jYWK1bt049e/ZU7dq1NWLECFubyZMnZ6jupKQkRUZG6saNG7p27Zp+//13ValSRSNHjtTJkyd14cIF7du3L1191qxZUxUrVtSaNWscAnBsbKy+/vpr9e3bV15eXvrzzz+1ceNGzZkzRy+88EKG3kOyCRMmqFy5ck73pXdN5JdffllLly5Vt27dtGrVKrtfkFu3btXgwYOVP39+XbhwwW5ZueQpB6nZtm2b7c/Lly/X2LFjVb58ebuf54cffpiuepNZrVZFRUXZfp4///yzzGazXn75ZZ08eVJhYWG2+ZFZbcyYMXYXhN49V1qSbd5rav/+k4N7auu5puSLL75QnTp17MJmZi1YsMBhdFX6/9UGZs2apS1btig+Pl5xcXGKj4/XrVu3dObMGZ05c8Y2v/jy5ctuq+luySPEWTkvNCIiQoGBgZn6ULVw4UJt375dX375pd0c37Fjx2rw4MEaN26cunTpoueee07PPfecpH++Wbp7+tjdEhMTNXz4cH3xxRd6//33bR8et2zZokcffVRNmjTRunXrXP6g4YrZs2fLy8sxupw5cybVdaZd0a9fPx08eNDuIulWrVrJz8/P9u1H8r+hnDi9CelDAEauVq9ePX3++ecaOHCgypYtqzp16igoKEiHDh3S8ePH1ahRI3311VcO603269cvzTnAK1asUIUKFVS4cGG999576tmzp6pWrWo3h/H9999PtY9jx46pZcuWSkhIUEJCgiwWi2JjYx3WNfX29lZ4eLh+//13VahQQY899liG1lvt0aOHpk6dqr///tvuPW/atEm3b9+2jcoljwzmz58/3cfISqVKldKGDRvUtWtXVa5cWXXq1FGRIkX0119/6dChQ6pcubI2btzoMC8ytV/SyVasWKHAwEA99thj+vDDD/XSSy+pZMmSdgF4/fr1qfYRFRWlKlWqyGKx2P08/71Gr6enp4oUKaKffvrJdjOTf3/FnVUmTZpkm9/6xBNPOOxPvtgrtZG95G82Uhq9S8nPP/+sEydOaPbs2Q77kkftEhISnAaYjITtOnXqqF+/fvr+++/1+++/y9PTU3ny5FFgYKAKFCigatWqqW3btipfvrxq1arldE3wtPzxxx8uXfgnSUOHDk1zSa0VK1bY1vtODrMzZ85Uvnz5ZLFYdOfOHd26dUvXr19XeHi4Ll68qBMnTigyMtJhjdv02rFjh8qUKeP0AreXX35Zn3zyibZv366KFSum2de5c+c0cOBA7dy5U++//77desa1atXSDz/8oM6dO6tatWqaMmWK0wuVc5rExESHf4f/3pYcgFNa5xi5BwEYuV6PHj30yCOP6LPPPtOvv/6q6OhoNW3aVDNnzlTHjh0zdBX1zp07dfjwYc2cOVOhoaFq166dVq9ene4ro4ODgzVq1ChZrVb5+PjIx8dHefLk0c8//6y5c+fqq6++Uv369VWoUCG3XO3do0cPTZo0SV999ZUGDBhg275q1SqVLVtW9evXlyRVr15djz76qIYNG6aYmBj17ds3x1zU8eijj+r48eNavHix9u7dq8jISNWsWVNjxoxRt27dMlRnWFiYNm3apKFDh2rIkCGqXr26Zs+erZdeeild/fj5+Wn06NFKSEiQt7e37ed56tQpTZw4UR999JE6d+6swoUL35O5xs6UKFHCNs3A2Shv8mhc8jQJZw4dOiQvLy+dPn3abnWNyMjIVFdLWLZsmby8vBxu6CL9f/AuW7as01HM8+fPp/sDmaenpz777LN0vaZIkSKqWrWq8uXL51L7hx56SMeOHcvwDUD+7e5vNbp06aL169dr9uzZ8vX1lZ+fn+3h7++v4OBgVa9eXaVKlVLFihUzvJ55sjx58igqKkpWq9XhfBMZGSnpn3nLx44ds/37CAsLc9rXkCFDdPjwYW3atMnputzVqlXT/v379frrr6frhhc53dmzZ+Xp6anixYtndynIJAIw7gshISHpuuVr+fLlU/zaPDExUS+99JJCQkI0dOhQ5c2bV61atdLw4cPVqlWrdM39CgoKcno1dPKIQvIIs/TPxUH/vj1retearFSpkqpXr67Vq1fbAvCdO3e0ceNGh1uzbtq0SdOnT9fo0aM1adIkjRo1Sv/5z3+UN2/edB0zK+TPn18jR45M80ryZCVLltTDDz/sdGRRkkaPHi0/Pz+NHTtWxYoV06BBgzRx4kR17drV5TuISf+s2/riiy86bE+e3lC6dGnbyFBYWJjDXPOrV6+6HLyySqFChVStWjWtW7dOEyZMcNgfGRmpTZs2KSEhQa1bt3bY36hRI6f9WiwWrVq1Si1btnQ60jpo0CB5enrqxo0bTsOkj4+PW+Zfp6Vt27Zq27aty+09PT1dGhHNiKpVq6Z4UWBqkj9Mp1fPnj31+eefa8yYMZo+fbrtQ9qNGzc0YsQIBQYGqkOHDpo4cWKa05cWL14sLy+vVD8QBQUFOfQTFBSkokWL3rNvRNztr7/+UpkyZXLMgAEyjgCM+8bx48ddvgtQ7969HW4vmuz111/X77//rtWrV9vC4Lx581S9enX17NlTGzZsyJLRvdmzZ2vJkiWZ7qdHjx6aMGGC7cLAjRs3Kjo62uGiJF9fX02aNEkjR47Uhx9+qEmTJumtt97S6tWr1bhx4zSPkzyClLzSxr8lb8/oyPaZM2ec3v7XmWbNmumZZ55xGoDnz5+vb775Ru+8847tav8ZM2bom2++UZcuXbR79+4sCf3Lly93Oi2jXr166erHz89Phw8f1oYNG5SYmGibfnHx4kWFhYUpLCxMpUqV0ltvveVyn8lf1S9ZskR9+/a12/fqq68qMjJS3333nUOtqd3VbMuWLbp27Zp69erldH++fPmcfnhwh7feekuzZ89WeHh4mm3Xrl2rwYMH67fffkvXhx/pn6W7Uprv7oyHh4fKli2r77//3q1fmX/yyScZel3btm31xhtvaNq0aVqzZo3q1aunO3fu6Pvvv5fJZNLy5cv1wAMPaM6cObbpXZMnT9abb77p0JezDznXrl3TwoULtW3bNv3555+6ceOGTCaTgoODVbNmTXXs2FFPP/20028I0nL3+cbZRbeZPd+4auHChTn29vRIHwIw7gu3b99WpUqV0nURylNPPaUvv/zSbtu8efP01ltvacSIEXryySdt28uUKaOPPvpIffr00WuvvZausOGqzz77zGEZqPT8sk3Wo0cPjR07Vl9//bV69+6tVatWqW7duinejSooKEjjxo1Tjx491LJlS3Xr1k2nT59O8yLB5JUCatas6XRZsuRRvox+VVivXj2XLm5L9vDDD+vXX3+12/b111/rxRdf1BNPPGE3Ap4/f34tW7ZMrVq10oABA7Rq1aoM1ZiaiRMnaty4cXbbWrRoke5fnr1799a0adPUqVMnSf+EKh8fHxUsWFAlS5ZU5cqV1bJly3T1+eyzz2rVqlV69tlndeLECT3++OOKi4vTggULtHz5co0aNcpp2E0tXCxbtkx58+ZV586d0zz+uXPnVLhwYbeNAt64cUOXLl1yqW1kZKSuXbuWoWWzSpcuna7pEL/88ov69u2rffv2qX379qm2nTVrlsaPH+/Stz5ms1kDBgzQRx995FIdd5s8ebK6deumefPm2ZZBGz58uIYNG2abqmAymWwfJl1dcnDz5s3q2bOn8ufPr/79+2vEiBEqVqyY4uPjdf78eW3atEkvvviipkyZorVr17o8rzpZ8vkmf/78TmtKPvdn9dSE5HnmyP0IwLgv+Pv7p+se8F27dnW4cUJMTIymTp2qXr16adasWQ6veeaZZxQeHq7atWtnuE6LxSJPT0/VrFlTw4cPtxvJuPuXTmaUKlVK9evX15o1a9S5c2dt2rTJpcBerlw5DR8+XCNHjtShQ4ccbjf8by1atNDPP/+siIgIp4HAZDKpYMGCatiwYYbeR3qu2H/55Zf1zjvvyGKx2H01OXHiRDVp0kTLly93eM2jjz6qBQsWZOqrzISEBJlMJpUtW1YjRoxw+OX7759nRkan3nzzTb3xxhuKj4+X2WxOsd7k5c1c4enpqY0bN2rkyJGaOXOmbUm5/Pnza+bMmemeGx0ZGamvv/5aTz75ZJqj6efOnUt1ybC7lShRQj169FCJEiVcqsOVW5y7GpSd8fDwSNd0iOQPcK58MF+/fr1KliypL7/8MtVvmKxWq1544QVt2LAhQwFYkqpUqZLuFVpSc+HCBT311FNq2rSp1qxZ4zDHu0GDBurWrZumTZumli1b6vHHH9fx48fTFST79++vcuXK6ebNmymeb0JCQlSnTp1Mvx8YAwEYhpQ3b16Hq339/Py0f/9+FSxYMMVRj7uXl8qI0qVLq23btlqwYEGaK0hI/6yZmpHlpJJHgVeuXCmLxaLu3bu79LrkEVRXL0ZK79f5WSU5dCUmJtoFxJ07d8psNqc40tivX79MHbdZs2by8/PT9u3b9d5776XZPjQ01Lb2a3pkdM5navLkyaOPP/5Y77zzjk6dOiU/Pz+VKVMmQx/C1qxZo5iYmBTX/r1bepYMq1ChglasWOFyHZldSzstMTExatasmfbt2+fyB+48efKoSpUqabazWCwqXLiwHnrooTTbFi9eXEeOHHHp+M58+eWX+vTTT/X111+n+QGwWrVqeuqpp1Jtt27dOt25c0czZsxIdZm2kJAQTZs2TZ07d9a2bdtc+rYgmYeHR6pTcNLj66+/trsT4L85+6Ca2ofXn376yXaBMXIPAjBwl4wsk5QeyUtnuSqtZblS0q1bN40aNUpjxoxxelHSd999p3fffVePPfaYihcvrujoaK1bt05r165Vt27dVLZs2QwdN6dJXic2q6T355nREbuslC9fvnR/Hf1vy5YtU3BwsNO1eu8lV6YmLF68OMNLiR07dky//PKLZs2apccffzzN9p6engoJCckRF5be7a+//tLWrVsVFxeXZgDu0qWLunTpkmqb5A8zrqxRnPx34a5VNTKiVatWOnr0qFv6Sv4GCLkPARi4DxUtWlQtW7bU1q1bnf6yz5s3r86fP68xY8YoNjZWAQEBqlChgubOnZvm19LA3S5evKjvvvtOL774YrYt/XavJM/frlChgltv7pDbtW/fXq+88oqmTZumRYsWpThaGh0drcmTJ6tQoULpnrfuTmazmZ8fCMBAZpjN5jQvFrubt7e3Ll265NJcxWQmk0nly5d3+WKUZKnd6KNOnToZWn7pfpeRn+f169d19OhRl+f3mkwmlS5d2u1TGry9vWUymdJVvzssX75cSUlJLk1/kGR736dOnXL5/0HyKFtK0zOSg/evv/6a5jJzyevaZiSsJ0+lOXr0aLouUC1QoECa3y55eXkpIiJCf/75Z5r/18+ePZupfz/Jo76///67yzc7yZ8/f4p3PytbtqxtZP3YsWMaOHCgqlWrpuDgYFksFl28eFHbt2/XkiVLlJCQoK+//trudsNAtrACBtS/f39r0aJF3d5vnjx5rJMnT05x/8iRI62+vr5WSel6HDlyxO21ZpdPPvnEKsl66tQpt/U5adIkqyRrXFyc2/q0Wq3W0NBQ64ABA1Lc/+6771rz5s2b7p/npk2b3FpnRutPr4YNG1ofeeQRu2116tSxhoaGutxHYmKitXnz5lYvL690/Z3t3r07xT43b95sDQwMdKkfDw8Pa82aNa2xsbHpfv+xsbHWOnXqWD09PdNV+xNPPJFm31OnTrWazWaX+jObzdaRI0emu/5kv/zyizU4ODhd76FDhw5p9nvq1CnriBEjrJUrV7b7+ebJk8dav35961tvvWW9efNmhuvOiNdee81qMpmsCQkJ9/S4yPlMVms2TsQBssmtW7cUExOj4OBgt/fr5+fn9tG9+0lCQkKadxRLr+joaN24ccO2VJK73L59W15eXrl20X5315+81mpOm9OKnCcpKUm3b9+WyWSSv79/lq/Pm5L4+HjduXMny68HQO5DAAYAAIChpG9SIQAAAJDLEYABAABgKKwC4aKkpCSFh4crX7582TaXCQAAACmzWq2KiopSSEhIqiuqEIBdFB4enuX3GAcAAEDmnT9/PtULownALkpeW/L8+fMKCAjI5moAAADwb5GRkSpevHiaa4ITgF2UPO0hICCAAAwAAJCDpTVdlYvgAAAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYChe2V0AAAC4f1mtVkVHR9ue582bVyaTKRsrAgjAAAAgC0VHR6tTp06251999ZX8/f2zsSKAKRAAAAAwGAIwAAAADIUADAAAAEMhAAMAAMBQCMAAAAAwFAIwAAAADIUADAAAAEMhAAMAAMBQCMAAAAAwFAIwAAAADIUADAAAAEMhAAMAAMBQCMAAAAAwFAIwAAAADIUADAAAAEPJ0QF4xowZ8vT01L59++y2R0ZG6q233lKNGjXk7++vBx98UIMGDdKVK1cc+khKStLkyZNVokQJ5cmTR/Xq1dPOnTvv1VsAAABADpMjA3BiYqKGDh2qVatWKSkpSRaLxW7/77//rn379mn69On6888/tX79eh05ckQtW7ZUYmKiXdvRo0drxYoVWrFihU6ePKlnnnlGHTp00C+//HIv3xIAAAByCK/sLsCZt99+W2FhYdq9e7cCAgIc9j/yyCN65JFHbM9LlCih1atXq3jx4vr555/VqFEjSdL58+f14Ycf6sCBA6pSpYok6YUXXtCZM2f02muvaceOHffmDQEAACDHyJEBeNiwYRo9erR8fHxcfk2xYsVUoEABXb161bZtw4YNqlGjhi38JuvXr5+qV6+uyMhIpwFbkuLi4hQXF2d7HhkZmc53AQAAgJwoR06B8Pf3T1f4laSzZ8/q+vXrql69um3bgQMHVKtWLYe2Dz30kLy9vXXo0KEU+5s+fboCAwNtj+LFi6erHgAAAORMOXIEOCNmzJihJ598UqVLl7ZtCw8PV926dR3amkwmFS5cWOHh4Sn2N3bsWI0aNcr2PDIykhAMAG50c8uc7C4B90B0bLzd81s7PlaCb/oGuZD7BLUZlt0lpOq+CMC7d+/W559/rv3799ttj4uLS3Ek2dfXV7GxsSn2aTabZTab3VonAAAAsl+OnAKRHpcuXVKvXr00d+5cVahQwW6f2WxWfHy809fFxsbKz8/vXpQIAACAHCRXB+CYmBh17txZ3bp1U9++fR32BwcH69KlSw7brVarrly5oiJFityLMgEAAJCD5NoAnJSUpN69e6tw4cKaOXOm0zZVq1Z1mBYhSUeOHJHFYlFoaGhWlwkAAIAcJtcG4JdeeklnzpzRypUr5eHh/G20b99e+/fv1+HDh+22L168WI0aNVLBggXvRakAAADIQXLlRXAfffSRvvjiC3333XeyWCy6efOmbZ+fn5/t4rXy5curf//+6tatmxYuXKgyZcpozZo1+vDDD7Vly5Zsqh4AAADZKcePAHt5ecnLyz6nL1y4UFeuXFFoaKjy589v9xg2zH7ZjY8++kidO3dW165dVapUKX366adavXq1mjVrdg/fBQAAAHKKHD8CbLFYHLY5m9ebEh8fH02bNk3Tpk1zZ1kAAADIpXL8CDAAAADgTgRgAAAAGAoBGAAAAIZCAAYAAIChEIABAABgKARgAAAAGAoBGAAAAIZCAAYAAIChEIABAABgKARgAAAAGAoBGAAAAIZCAAYAAIChEIABAABgKARgAAAAGIpXdhcAAADuX3nM3lo67HG750B2IwADAIAsYzKZlNfXJ7vLAOwwBQIAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABgKARgAAACGQgAGAACAoRCAAQAAYCgEYAAAABhKjg7AM2bMkKenp/bt2+ewLyYmRsOHD1dwcLD8/f3VvHlzHThwIMPtAAAAYAw5MgAnJiZq6NChWrVqlZKSkmSxWBza9OrVS/v27dPmzZt17NgxNWnSRM2aNdO5c+cy1A4AAADG4JXdBTjz9ttvKywsTLt371ZAQIDD/h9//FFbt27V6dOnVbhwYUnSpEmTdOTIEU2ePFkLFy5MVzsAAAAYR44cAR42bJg2b96sfPnyOd2/bt06Pf7447ZQm6xfv37asGFDutsBAADAOHJkAPb395ePj0+K+w8cOKBatWo5bK9Vq5auXr2qixcvpqudM3FxcYqMjLR7AAAAIPfLkQE4LeHh4SpatKjD9uDgYNv+9LRzZvr0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5s1re75u3To9/vjjtvCbrF+/fhowYIBbawIAAEDOl66L4AICAjR58mS9/PLL+u9//6utW7dq+PDhunLliqxWqyTJbDarUqVKatWqlV577TU1bNgwSwrv16+fGjZsqIMHD6p69eqSpMuXL+u9997Te++9Z2t34MABtWjRwuH1tWrV0tWrV3Xx4kUVK1bMYX9cXJzi4uJsz909fxkAAADZI0OrQOTLl08DBw7UwIEDJUmJiYm6evWq8uTJo4CAALcWmJLKlStr5cqV6tatm8aMGaMSJUpoxIgRmjhxojp16mRrFx4e7nSkODg42LbfWQCePn26Jk2alHVvAAAAANnCLcugeXp62gLlvVSjRg01adJEn376qfLnz6+iRYuqbt26dm3i4uLk4+Pj8FoPDw95e3srNjbWad9jx47VqFGjbM8jIyNVvHhx974BAAAA3HMu3wkupzl48KAaNGigdu3aae/evfrmm280btw4tWvXTl988YWtndlsVnx8vMPrk5KSZLFY5Ofn57R/s9msgIAAuwcAAAByv1wbgIcNG6ZBgwbpiSeesG1r2rSpPvvsMw0ePNi2dnFwcLAuXbrk8PqIiAhJUpEiRe5NwQAAAMgRcm0A3rdvnx5++GGH7fXq1dOdO3d07NgxSVLVqlW1f/9+h3b79+9XUFCQHnzwwSyvFQAAADlHrg3AxYsX17Zt2xy27969W5JsF7517NhRmzZt0tWrV+3aLV68WB06dJDJZMr6YgEAAJBjuOUiuOwwdepUde/eXVarVYMGDZK/v7++//57jR49Wv369bON7LZo0UINGzZUly5dNGfOHBUqVEgLFizQli1b9Ntvv2XzuwAAAMC9lmtHgJ988knt2rVLf/31l5o1a6bQ0FC99957Gj9+vBYsWGDXds2aNapSpYpatmypcuXKaceOHdq+fbsqVaqUTdUDAAAgu+TaEWBJatKkiZo0aZJmu4CAAM2bN0/z5s27B1UBAAAgJ8u1I8AAAABARrg0Avzee+/JYrFk+CDe3t4aOXJkhl8PAAAAuItLAXjevHmZCsA+Pj4EYAAAAOQILgXgsLCwrK4DAAAAuCeYAwwAAABDIQADAADAUNK9DNq5c+e0ePFi/fDDDzp37pysVqsefPBBNWnSRH369FHZsmWzok4AAADALdI1Ajx79mxVqFBBs2bNUkBAgNq1a6dOnTqpUKFCmjt3rkJDQ/Xmm29mVa0AAABAprk8ArxmzRq98sormjVrlgYNGiRfX1+7/RaLRcuWLdOLL76oAgUK6Pnnn3d7sQAAAEBmuRyA3377bb355pt64YUXnO739vZW//79ZTab9dJLLxGAAQAAkCO5PAXiyJEj6tChQ5rtOnfurMuXL+vatWuZKgwAAADICi4H4Dx58ujGjRtptrt+/bokKW/evBmvCgAAAMgiLgfgFi1aaOrUqUpKSkq13fjx49WwYUP5+fllujgAAADA3VyeAzx9+nTVrVtXDz/8sAYPHqwWLVooJCREnp6eunLlinbt2qWFCxfq4MGD2r17d1bWDAAAAGSYywG4VKlS+vXXXzV69GgNGzZMiYmJdvs9PDzUpk0b/e9//1OlSpXcXigAAADgDum6EUbJkiW1evVqRUZG6s8//9SFCxeUlJSkYsWKKTQ0VPnz58+qOgEAAAC3SPed4CQpICBA9evXd3ctAAAAQJZL153gnFmzZo0sFos7agEAAACyXKYDcLdu3XT+/Hl31AIAAABkuUwHYKvV6o46AAAAgHvCpTnAK1asSHGag8lk0rp161SoUKEUX+/t7a2ePXtmrEIAAADAjVwKwIMHD04xAPv4+GjcuHGpvt7Hx4cADAAAgBzBpQAcGRmZ1XUAAAAA90Sm5wD/25UrV9zdJQAAAOA2bg3AFotFTZo00bx589zZLQAAAOA2GQrAv/zyiyIiIhy2v/rqq0pKSlK/fv0yWxcAAACQJVwOwGfOnJEkxcXFqXHjxipTpoxeeeUVxcTESJIWLFigBQsWaO3atfLz88uSYgEAAIDMcjkAly1bVsePH9etW7eUkJCgt99+W8uWLVOdOnX05ptvavjw4fryyy9VtWrVrKwXAAAAyBSXA7DVarU9TCaTBgwYoCNHjig0NFQTJ07UlClT1Lp166ysFQAAAMi0TF0Elz9/fv33v//VkCFDNG3aNB09etRddQEAAABZwqV1gNMyd+5cxcbGqlOnTvrtt98UEBDgjm4BAAAAt0vXCLDJZEpx39y5c5UnTx699tprmS4KAAAAyCouB+C9e/eqTJkyKe739fXV0qVL9emnn+rPP/90S3EAAACAu7kcgBs0aCBPT0/5+vqqTp068vb2dmhTrVo19ezZU6NGjXJrkQAAAIC7pHsOcGBgoH7++ecU90+bNi3V/QAAAEB2cuutkCUpODhYnTt3dne3AAAAgFu4PQADAAAAORkBGAAAAIZCAAYAAIChEIABAABgKARgAAAAGEqGAvCsWbN0584dd9cCAAAAZLkMBeBXXnlFERER7q4FAAAAyHIZCsBWq9XddQAAAAD3hEt3gps+fbosFovdtg8++EAFChSwPe/Zs6dWr17t0M5kMqlOnTpq06aNG8oFAAAAMselALxixQq7YFuxYkVt3brV9txkMqlhw4ZauXKl4uPjZbFYdPr0aVWoUEHR0dGaNWuWbt686fbiAQAAgPRyKQD/8ccfLnV28OBBSdKJEydUsWJFHT16VGFhYapcuXLGKwQAAADcKNPLoA0ZMkTr16+322YymWx/9vT0zOwhAAAAALfJdAC+ffu29uzZ445aAAAAgCyX6QBcpkwZnThxwh21AAAAAFku0wG4dOnSunjxojtqAQAAALJcpgNw8eLFdeXKFXfUAgAAAGS5TAdgX19f3bp1K8X93DQDAAAAOUmmA7CPj49u376d4v67V4QAAAAAsptL6wCnxmw220Z5W7duLYvFopiYGFmtVjVv3ly3bt2Sr69vpgsFAAAA3MHlAHzgwAGH2xxL0u+//25b67dixYq2NjVq1JAkeXh4aNiwYW4oFQAAAMg8lwPwww8/LMn5nN7WrVtLkj744AM3leW6c+fOadq0adq6dasuXbqkvHnzqnfv3po9e7YkKSYmRq+++qpWrVql27dvq27dupo1a5Zq1qx5z2sFAABA9nM5AJ87d04JCQkO2729vRUSEuLWolz1888/q3379ho4cKDWrl2rYsWK6ebNm7p+/bqtTa9evXTlyhVt3rxZhQoV0oIFC9SsWTMdOnRIJUqUyJa6AQAAkH1cDsDFihXLyjrSLTY2Vt26ddPcuXPVvXt32/bChQvb/vzjjz9q69atOn36tG37pEmTdOTIEU2ePFkLFy6853UDAAAge2VqFYhr1665q450W716tQoVKmQXfv9t3bp1evzxx+1CsST169dPGzZsyOoSAQAAkANlOACfPn1awcHB7qwlXbZv36527dpp3bp1qlevnh588EG1aNFCW7dutbU5cOCAatWq5fDaWrVq6erVq6newS4uLk6RkZF2DwAAAOR+GQ7ASUlJSkpKcmct6XL06FH98MMPmjJliqZNm6bNmzerbdu26tSpkz7//HNJUnh4uIoWLerw2uTgHh4enmL/06dPV2BgoO1RvHjxrHkjAAAAuKdcmgMcHh7ucAHcxYsXZTKZdP78eYeVIZJDoySdP39e5cuXV2xsrJtK/sfNmzd14cIFhYWFyd/fX5JUtWpVJSYm6tVXX1Xv3r0VFxcnHx8fh9d6eHjI29s71ZrGjh2rUaNG2Z5HRkYSggEAAO4DLgXglIKf1WpVqVKlHLb36NFDX3zxhSQpPj5e8fHxGa8wFV27drWF32Tdu3fXq6++qlOnTslsNjs9dlJSkiwWi/z8/FLs22w2y2w2u71mAAAAZC+XAvCvv/7q9CYYKfn33OCsuB1y/vz5nc5BTt5269YtBQcH69KlSw5tIiIiJElFihRxe10AAADI2VwKwM4uJMtulStX1smTJx22X7hwQdI/Qbhq1arav3+/Q5v9+/crKChIDz74YJbXCQAAgJwlU8ugZafHH39c//3vf/X333/bbV+6dKlq1KihkJAQdezYUZs2bdLVq1ft2ixevFgdOnTIkpFpAAAA5Gwu3whj+vTpKU6DMJlMqlOnjtq0aeO2wtLy5JNPasaMGercubM+/PBDFSpUSKtXr9a7776rjRs3SpJatGihhg0bqkuXLpozZ47tTnBbtmzRb7/9ds9qBQAAQM7hcgD+8ssvbQH46tWrslgstlsgR0VFadasWbp582aWFOmMp6entmzZotGjR+vRRx9VTEyMHn74YW3cuFFNmza1tVuzZo3GjBmjli1b6vbt26pdu7a2b9+uSpUq3bNaAQAAkHO4HIDvnks7depUnTx5UosWLZIknTp1SuXLl3d/dWkoXLiwbc3flAQEBGjevHmaN2/ePaoKAAAAOZlb5gB7enq6oxsAAAAgy+Xai+AAAACAjHB5CoSrfvjhB3322We259euXZOvr6+7DwMAAABkiNsDcHR0tN3NJzw9PTVlyhR3HwYAAADIkAwtg7Z7925dv35dkydPliSFh4fb5gG3adPmni6HBgAAAKRHhpZBu3ubJHl4eGjQoEHurQwAAADIAhlaBg0AAADIrVxeBSIxMTEr6wAAAADuCZcDcP78+dW7d2998803SkhIyMqaAAAAgCzjcgCeMmWKwsLC1LFjRxUuXFgDBw7Uzp07ZbVas7I+AAAAwK1cDsAvvviifvnlF504cUKjRo3SL7/8opYtW6po0aIaNmyY9u7dm5V1AgAAAG6R7jvBlSlTRuPGjdOhQ4d08OBBDRw4UJs3b1aTJk1UokQJvfzyy9q3b19W1AoAAABkWqZuhVy1alVNnTpVJ06c0M8//6wnn3xSK1asUN26dVW+fHm98cYbOnz4sLtqBQAAADItUwH4bnXr1tV7772n8+fPa+fOnWrRooXmz5+v6tWrq0qVKtq+fbu7DgUAAABkmNsCcDKTyaRmzZpp/vz5ioiI0DfffKPatWsrLCzM3YcCAAAA0s3lG2FkhKenp9q2bau2bdtm5WEAAAAAl7l9BBgAAADIyVwaAb5+/Xqmbn7h5eWlAgUKZPj1AAAAgLu4FIALFSqU4QNYrVZ5eHhw9zgAAADkCC4F4D179ig+Pl6SFBcXpzZt2mj9+vUKDAy0tbl06ZKefvpp7dq1y+H13t7ebioXAAAAyByXAnCDBg1sf46Li5PJZFLjxo3tpjWcPXtWJpNJTZs2dX+VAAAAgJtk6CI4q9Xq7joAAACAeyJDy6CZTCbdvn1b586dU1RUlPLly8c0BwAAAOQK6Q7AGzdulNVqVenSpSX9MxpsMpls+3/66Se7KRMAAABATuJyALZYLOrTp482btyo/v37q1OnTipZsqS8vLx07tw5bdy4UatWrdKjjz6q7777TvXr18/KugEAAIAMcTkAv/POO/rtt9905MgRFS9e3G7fQw89pLZt22ry5Mlq3769Ro0apR9//NHtxQIAAACZ5fJFcEuXLtWMGTMcwu/dChQooHnz5ul///ufoqOj3VIgAAAA4E4uB+CzZ8+qfPnyabarVKmSrFar/v7770wVBgAAAGQFlwPwgw8+qP3796fZ7tdff5XZbFZwcHCmCgMAAACygssBuH///nrllVf0v//9L8U2J06c0KBBg9SrVy/5+Pi4pUAAAADAnVy+CG7MmDE6ePCgGjZsqFq1aqlVq1YqVqyYvLy8dPnyZe3atUt79uxRkyZN9MEHH2RlzQAAAECGuRyAPTw8tHLlSj3zzDNaunSp1qxZo4sXLyoxMVEhISGqUqWKli1bpq5du9qtCwwAAADkJOm+EUa7du3Url27rKgFAAAAyHIuzwEGAAAA7gcEYAAAABiKS1Mgdu3aJYvFkuGDeHt769FHH83w6wEAAAB3cSkAt27dWgkJCU73mUwmWa3WVF/v7e2tuLi49FcHAAAAuJlLUyDi4+OVlJTk9GG1WnXixIkU9yclJRF+AQAAkGO4ZQ4wy54BAAAgt3BpCsSECRNSnQM8c+ZMBQUFpbjf29tbkyZNSndxAAAAgLu5FIB//vlnxcfHO93XtGlTHT16NNXXm83m9FcGAAAAZAGXAvDWrVtd7vCvv/6Sv7+/ihUrluGiAAAAgKzi9nWAp02bpo8//tjd3QIAAABu4XIADg8P10MPPZRmu/Lly+vQoUOZKgoAAADIKi4H4JiYGB07dizNduXLl9fZs2czVRQAAACQVdw+BSIkJER///23u7sFAAAA3MLtAdjLy0uxsbHu7hYAAABwC7cHYE9PTwIwAAAAciyXlkGT/v9ubydOnJCPj0+K7U6ePJnqTTMAAACA7ORyAC5SpIh8fX1VsWLFVNtZrVY1aNAg04UBAAAAWcHlAJw3b16dOnVKZ8+eldVqTbGdj4+PQkND3VIcAAAA4G4uB2Dpn1HgIkWKZFUtAAAAQJZz+0VwAAAAQE5GAAYAAIChEIABAABgKARgAAAAGMp9E4B/+ukneXl56dlnn7Xbfv36dfXp00cFCxZUYGCgOnbsqNOnT2dTlQAAAMhu90UAtlgsGjx4sBo0aGB3E47ExES1bt1aUVFR2rNnj37//XcVLVpUTZs2VWRkZDZWDAAAgOxyXwTgWbNmqUqVKmrRooXd9lWrVikiIkIrVqxQ5cqVVbp0ac2fP19FihTRBx98kE3VAgAAIDvl+gB8+vRpffDBB3rvvfcc9q1bt049evSQr6+vbZvJZFLfvn311Vdf3csyAQAAkEPk+gA8dOhQvfHGG05v0HHgwAHVqlXLYXutWrX0xx9/KCkpKcV+4+LiFBkZafcAAABA7perA/CKFSt048YNDR482On+8PBwFS1a1GF7cHCw4uPj9ffff6fY9/Tp0xUYGGh7FC9e3G11AwAAIPvk2gB88+ZNvfzyy/r444/l4eH8bcTFxcnHx8dhe/KUiNjY2BT7Hzt2rG7dumV7nD9/3j2FAwAAIFt5ZXcBGTVmzBh17dpVNWrUSLGN2WxWfHy8w/bk4Ovn55fqa81mc6brBAAAQM6SKwPwzz//rM2bN+vIkSOptgsODtalS5cctkdERMjb21v58+fPqhIBAACQQ+XKAPzjjz/q8uXLDvNyY2NjlZSUpPXr1+uNN95Q1apVtX//fvXs2dOu3f79+xUaGipPT897WTYAAABygFwZgIcMGaIuXbo4bH///fd14cIFvfPOO3rggQcUFBSkiRMn6s0337TN+7VarVq6dKk6dux4r8sGAABADpArL4LLkyePSpUq5fAICgqSv7+/SpUqJX9/f/Xu3VuBgYHq2bOn/vrrL505c0ZDhgzR+fPnNWzYsOx+GwAAAMgGuTIAp8TX19fuphdms1nbt2+Xr6+v6tevrypVqujcuXPatWuXChUqlI2VAgAAILvkyikQKXn11VcdthUtWlQrVqzIhmoAAACQE91XI8AAAABAWgjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDybUB+PLly3rjjTdUuXJl5cmTR6VLl9ZLL72kqKgou3YxMTEaPny4goOD5e/vr+bNm+vAgQPZVDUAAACyW64NwN9++63Cw8P10Ucf6fjx41q8eLG++eYb9ezZ065dr169tG/fPm3evFnHjh1TkyZN1KxZM507dy6bKgcAAEB2MlmtVmt2F+EuP/30kxo2bKgLFy6oWLFi+vHHH9WyZUudPn1ahQsXtrV76qmnFBQUpIULF7rcd2RkpAIDA3Xr1i0FBARkRfmpWv3L1Xt+TAD3xlN1C2V3Cdni5pY52V0CgCwS1GZYthzX1byWa0eAnalataok6erVf8LiunXr9Pjjj9uFX0nq16+fNmzYcM/rAwAAQPa7rwLwvn37lCdPHlWoUEGSdODAAdWqVcuhXa1atXT16lVdvHgxxb7i4uIUGRlp9wAAAEDud18F4BkzZui5555Tnjx5JEnh4eEqWrSoQ7vg4GDb/pRMnz5dgYGBtkfx4sWzpmgAAADcU/dNAF62bJkOHDigV1991bYtLi5OPj4+Dm09PDzk7e2t2NjYFPsbO3asbt26ZXucP38+S+oGAADAveWV3QW4w5EjRzR8+HB9+eWXKliwoG272WxWfHy8Q/ukpCRZLBb5+fml2KfZbJbZbM6SegEAAJB9cv0I8LVr19ShQwdNnDhRzZs3t9sXHBysS5cuObwmIiJCklSkSJF7UiMAAAByjlwdgGNjY9WxY0e1adNGw4Y5LrdRtWpV7d+/32H7/v37FRQUpAcffPBelAkAAIAcJNcGYKvVqt69eyt//vyaM8f5WpIdO3bUpk2bbMuiJVu8eLE6dOggk8l0L0oFAABADpJr5wCPGTNGhw8f1o4dOxxuf5w3b155e3urRYsWatiwobp06aI5c+aoUKFCWrBggbZs2aLffvstmyoHAABAdsq1I8ALFy7UX3/9peLFiyt//vx2j5kzZ9rarVmzRlWqVFHLli1Vrlw57dixQ9u3b1elSpWysXoAAABkl1w7Anz9+nWX2gUEBGjevHmaN29eFlcEAACA3CDXjgADAAAAGUEABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYCgEYAAAAhkIABgAAgKEQgAEAAGAoBGAAAAAYiiEC8PXr19WnTx8VLFhQgYGB6tixo06fPp3dZQEAACAb3PcBODExUa1bt1ZUVJT27Nmj33//XUWLFlXTpk0VGRmZ3eUBAADgHrvvA/CqVasUERGhFStWqHLlyipdurTmz5+vIkWK6IMPPsju8gAAAHCP3fcBeN26derRo4d8fX1t20wmk/r27auvvvoqGysDAABAdvDK7gKy2oEDB9SlSxeH7bVq1dLo0aOVlJQkDw/HzwFxcXGKi4uzPb9165YkZdu0iTu3o7LluACyXmSkObtLyBaR0THZXQKALOKRTXkpOadZrdZU2933ATg8PFxFixZ12B4cHKz4+Hj9/fffKlSokMP+6dOna9KkSQ7bixcvniV1AgAA3D/GZOvRo6KiFBgYmOJ+kzWtiJzLeXp66ocfflDDhg3ttoeHh6tYsWI6d+6c01D77xHgpKQkXb9+XQULFpTJZMryumFckZGRKl68uM6fP6+AgIDsLgcAMo3zGu4Vq9WqqKgohYSEOP2GP9l9PwJsNpsVHx/vsD02NlaS5Ofnl+LrzGb7ryWDgoLcXh+QkoCAAH5RALivcF7DvZDayG+y+/4iuODgYF26dMlhe0REhLy9vZU/f/5sqAoAAADZ5b4PwFWrVtX+/fsdtu/fv1+hoaHy9PTMhqoAAACQXe77ANyxY0etXLnSNuVB+md+yNKlS9WxY8dsrAxwzmw2a8KECQ5TcAAgt+K8hpzmvr8ILi4uTrVr11b58uU1Y8YMmc1mTZ8+XRs2bNAff/zhdAUIAAAA3L/u+xFgs9ms7du3y9fXV/Xr11eVKlV07tw57dq1i/ALAABgQPf9CDAAAABwt/t+BBgAAAC4GwEYAAAAhkIABlJx4cIFeXt7O91Xvnx57dmzx/Z84MCBmj59ul2bc+fOqVu3bgoMDFRQUJCefPJJHT9+3K7N2bNn5evrq8TERKfHGTBggCZMmOCwvVy5cjpw4IAkKX/+/AoPD7ftu3Xrljw8PGQymRweDz30kN3NYcqVK2f3PgAgISFBb731lkqVKiVfX1/VrFlTy5cvd2jXvHlzLVu2zGH77t27VbJkSYftU6ZM0YgRIyRJw4cPdzhnPvnkk07PW/7+/tq2bZtdP4MGDcrku4SREYCBVCQkJCghIcHpPovFYrfv37fPvnz5smrXrq2QkBDt2bNHP/30k6pUqaIGDRrYhWCLxaK4uDilNB0/JiYmxdqSjxcTE2MXagMDA2WxWBwe0dHROnr0qF1Yjo2NtVsmEAD69++v//73v/r000/1119/ady4cRo3bpzefvttu3bx8fFOzx+pnbeS2//7nClJq1evdnru6tixo3788UdbO85byKz7/lbIQHYZP368Hn30Ub3//vu2bZMmTVJsbKwqVKjgcj+xsbEpjkKnxtlNXkwmk6xWq7y8+K8PwLmffvpJX331lcLCwhQcHCxJKlmypCpWrKjq1atrzJgxdu379evn0EdGz1smk8np+cnT05PzFtyKEWAgi+zdu1ft27d32P7EE0/Iz89PVqtVVqvVYUrEv12/ft1tt+y+fPmyJKlgwYJu6Q/A/Wfv3r16+OGHbeE3WZUqVVS+fHmtWrXKdv5q1KiR0z7ced6S/jl3PfDAA27rDyAAA1nEarXKZDI53Zc8EusKd574T58+reDgYPn5+bmlPwD3n9TOXa5yd2A9ffq0Spcu7bb+AL5PAFyQkV8GjRs31jfffKNnnnnGbvuXX34ps9mskSNHSvrngrWUJCUl6fTp07Y5u0lJSba5vuldwjsxMVG//PKLChUqpC+++ELHjh1z+tUlAGNr1KiR3nzzTV2+fFlFihSxbT948KCOHz+utWvX2ubjnj592mkfJ0+etLvWIPk6h5SuqUjNlStXdOrUKZ05c0azZs1SSEhIuvsA/o0RYMAFyV/33f1wdoXz3d58803t2rVLL730ko4fP67Tp09r/Pjxmjt3rgYNGqSgoCAFBQUpX758KfZx6NAhxcfHa+/evZL+mUPs5+cnPz8/nTt3zulrtm3bpho1aqhatWqqVKmSSpYsqQIFCsjPz0/vvPOOfH19tXHjRnl5eSlPnjwZ/0sBcF9q2LChnnjiCbVv314//PCDIiIitH79enXs2FFt2rRRpUqVbOcvZ9caSNJvv/2mI0eO6ObNmzp16pR8fX3l5+enqVOnpnjc+vXrq0aNGgoNDVWZMmVUpEgRmc1mVaxYUZUrV9aGDRt08eJFFS5cOKveOgyEEWAgixQuXFj79u3T8OHDVaNGDSUlJalOnTraunWrmjZtamt34sQJzZkzx2kf27ZtU7FixbRp0yZdv35dkyZN0qRJkyRJpUqVcvqa+vXr65NPPpGXl5fMZrPy5MmjMmXKKCwsTOXLl3f7+wRw//n00081c+ZM9ejRQxERESpVqpSGDBmil19+2e5itB07dji89vLly/rjjz9UrFgxLV++XM8995ztG6uJEycqIiLC6THnz5+vhIQEmc1m+fr66rXXXlOFChWchuZdu3a56Z3CqAjAQBYqXry41q5dm2qbu9e6vFtSUpIWL16sUaNG6bvvvtPs2bNt4Tc1AQEBqlu3rtPjJMvI15AAjMPLy0tjx47V2LFjU21nMpnk4WH/ZfLixYv10EMPaeLEiXr11Vc1aNAg+fj4pHnMGjVq2D3Pmzev3XkrMTHRFqTTOwUM+DemQACpSD75/nutyuS5uK7MDU6e95bSo2TJkrpw4YLDV4lLlizR9evXNXjwYE2bNk3vvvuuwsLCMv2e5s2bJ29vb9vj4sWLme4TwP0ptXNXQkKC1qxZo169etnaX7t2TW+//bYmTJigJ554QgULFtSMGTMyXUdUVJR8fX1t561p06Zluk8YGwEYSEXhwoUVEhIiX19fu5FaT09PxcfHq0yZMmn20aNHD7vA6ezRuXNnu9ccPnxYI0aM0CeffKK8efOqSpUqGjVqlJ544glFRUW5XH9SUpKkf+6ulLzyw9ChQ+3mMhcrVsz1vxAAhrF58+Y0z11lypTRH3/8Iemfm/r06tVLjz76qO2ObslTKe6+i1taks9N9erVU8WKFSVJ+fLlk8Vise17/fXXs+Q9wzgIwEAq/Pz8dPHiRacXwV27dk3FixdPs4+718x09jhy5Ih+/fVXu1shd+vWTa+88oo6dOhg2zZ+/HhVr149xYvfnClXrpx++uknrV69OtWgm9kljwDcf9q2bZvquctqtSo0NFSHDh2SJL377ru6du2aFi1aZOsjNDRU8+bN0/79+10+7ptvvqkhQ4Zo6NChDqvo3I3zFjKDOcBANkueG3f3nLZdu3bZLT8k/XMnpOXLl6er7/j4eIfpG/+2detWlStXLl39AoD0z/kr+Zum4cOHa8iQIQoICLBr07t373T16cp568UXX7S7/TuQXgRgIAf6d/jNKA8PD0VHRys2NjbFNmXLluUWowAyzdfXV76+vpnux8PDQ7GxsametwICAhwuvgPSg996gJuYzWaZzeZ7djxfX1/b8Xx9fZ1eZV2/fn116NAhzSum+/TpoyVLlmRJnQCQ7O6QnNI5s1atWpo5c2aad6z09PRUTEyMvL29s6RW3N9MVtYSAbLV5cuX1bdvX23atIkRDQC5yksvvaTWrVurZcuW2V0KkC4EYAAAABgKw00AAAAwFAIwAAAADIUADAAAAEMhAAMAAMBQCMAAcA+0atVKb7zxhu25t7e3du7cmak+f/zxR3l4eOjatWuZLS/LeXt7a82aNZnu59lnn7W7QyIAZAQBGAAy6fbt2zp58qROnDhh97h48aKtTXx8vCwWi+15QkKC3fNkiYmJmj59usqXLy8/Pz9VrFhR06ZNc3rXq/j4eFmtViUkJLhca+PGjTVhwoR0vkPn9uzZo3bt2qlAgQLy8fFR6dKl9dJLL+nSpUsObVN6v5K0aNEimUwmu4ezda0lyWKxpNgPALiKG2EAQCZERESobNmyunPnjsO+SpUq6ejRo+nqr1evXtqxY4feeust1a5dW8ePH9fYsWP1xx9/aOXKlZmqNTIyUr/88otbRlA/+OADjRw5UoMGDdKqVav0wAMPKCwsTPPmzVNoaKi+/fZb1axZ06W+unfvroYNG9pt4+YGALISARgAMuHs2bO6c+eOjh07pooVK2aqr40bN2rVqlXau3evLRDWqFFD9erV00MPPaTVq1erffv2tvbORoVTs2zZMlksFi1evFgjRozI8J0Lb926pTFjxmjy5Ml6/fXXbdtr1qypp556Su3bt9cLL7ygvXv3ptnX33//LYvFoqCgILvtJpNJFouFIAwgSzAFAgAyIfleQu64DfaSJUtUq1Yth9HQEiVKqGvXruratav8/Pxsj9atW7vcd1RUlKZOnarBgwcrKipKkyZNynCdx48fV2xsrNORZE9PTz355JPav3+/S31VqFBBRYsWdXgEBwerQIECTqdTAEBmEYABIIc4ePCgWrVq5XRfp06dJEmnTp3S1atXdfXqVa1fv96lfpOSkjRgwACZzWa99957+vzzz/XOO+9o3rx5GaqzVKlS8vLySnGEd9u2bS6Phl+4cEFRUVEOjxdeeEF+fn4qWLBghmoEgNQwBQIAcoiLFy+qWLFiTveVLFlSknTjxg2VLl1akhQYGJhmnxaLRf/5z3+0c+dO7d69W35+fnr00Uf13//+Vz169NDZs2c1ceJE+fr6ulznAw88oNdff12jR49WbGysunbtqgIFCujo0aOaOXOm1q9fr02bNrnUl5+fn8O2EydOaOHChZoyZUqKF8MBQGYwAgwA98hbb71lW+XAmcTERHl5OR+XSN4eFRXl8vEOHTqkJk2aaNOmTdq0aZOqVKli29e5c2ft3LlTa9euVbVq1bRs2TLFxsa63PfEiRO1aNEiffzxxypevLjy5s2revXq6fLly/r+++/12GOPudzX3SwWiwYNGqTQ0FANGzYsQ30AQFoIwABwjwwePFjHjx/X8ePHne4PCQnR5cuXne47d+6cJClfvnxpHufChQt6+umnVbNmTeXLl0+///676tev79CuUaNGOnjwoHr16qVRo0YpJCREkZGRLr+fHj166NixY7p8+bJOnDihW7duadeuXWrQoIHLfdwtKSlJffr00ffff6/XX3+d0V8AWYYpEABwjwQFBalcuXIp7q9SpYp27drldJ3e5CkFPj4+OnPmjKR/lmBzxmq16vbt29q2bZuaN29ut69Vq1Zq0aKFxowZI+mfKQgTJkzQmDFjtGfPHgUEBKT7fT3wwAPy8/PTrVu3dPr0aV2+fFmXLl3ShQsXFB8fr/Hjx6fZx507dzRo0CDt2LFD/fr105AhQ1SmTBnVqFEj3fUAQFoIwACQQzz99NPq1auXwsLCVKFCBdv269ev29YArlq1apr9FC9eXBs2bHC678qVK7px44bDdl9fX5emLUyZMkXvv/++rFarLBaL4uLi7JZjCwgIUOHChRUSEqLSpUvbTbtIyZ9//qnu3bsrJiZG33//vUJDQzVu3Dg98sgjWrRokZ566qk0+wCA9CAAA0AmJH9N/+OPP+ry5ctKTExUQkKCwsPDbdMdXnvtNZf66tq1qz7++GO1b99en376qerUqaO//vpLQ4cOVWRkpH744Qc1btzY1v67777To48+miXvKyUDBw5U8+bN5eHhIV9fX/n6+uratWtq0qSJ9uzZo0aNGrnc15UrVzRp0iQtXLhQTz/9tN577z3besBTpkxRmTJl9Mwzz2jlypVasWIFawIDcBsCMABkQmhoqOrXr68BAwbI29vbFgoLFiyokiVLqmrVqvL393epLw8PD23YsEEvvviiHnvsMdvIat26dbVr1650hcuskrxO792Sp2R4enqmq6/Zs2fr4MGD2rt3rx5++GGH/QMGDFD9+vW1atUqwi8AtyIAA0Am+Pr66qeffnJbf/7+/lq0aJE++ugjRUREqGDBgi5d+Jbs+vXrun79eor74+PjdfPmTZ04cSLFNsHBwS6H9sx488035eGR+rXYoaGhmbppBwA4QwAGgBzI19dXpUqVSvfrRo4cqaVLl6ba5ujRo/r4449T3D9t2jSNHTs23cdOr7TCLwBkFQIwANxHlixZoiVLlmR3GQCQo/HxGwDuAR8fH7t5rF5eXpme1+rt7S2TyZTizTPuleT3kdq6ve54v8nHYj4wgMwyWa1Wa3YXAQAAANwrjAADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMBQCMAAAAAyFAAwAAABDIQADAADAUAjAAAAAMJT/A3MwyKm7FBsaAAAAAElFTkSuQmCC\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [],
      "metadata": {
        "id": "QqbLcqoXOZez"
      },
      "execution_count": null,
      "outputs": []
    }
  ]
}