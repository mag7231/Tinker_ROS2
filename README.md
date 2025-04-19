# Tinker_ROS2

## Обзор проекта

## Шаг 1: Проверка требований

Перед началом убедитесь, что у вас есть всё необходимое:

1. ✅ Ubuntu Linux, желательно `Ubuntu 24.04` (WSL тоже подойдёт)
2. ✅ Установлен и настроен **ROS2 Jazzy**
   Проверь:

   ```bash
   ros2 --version
   ```

3. ✅ Установлен **Anaconda или Miniconda**  
   Проверь:  
   ```bash
   conda --version
   ```
4. ⚠️ **NVIDIA GPU (желательно)**  
   Требуется для `IsaacGym` и обучения с помощью RL.

---

### 🔗 Полезные ссылки (если чего-то не хватает)

- Установка Ubuntu  — [Курс на Stepik](https://stepik.org/lesson/1505339/step/1)
- Установка ROS2  — [Курс на Stepik](https://stepik.org/lesson/1505346/step/1)

---

## 🐍 Установка Miniconda (в WSL Ubuntu 24.04)

Если `conda` ещё не установлена, сделай следующее:

```bash
# 1. Скачай установщик:
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# 2. Установи:
bash Miniconda3-latest-Linux-x86_64.sh
```

Просто нажимай `Enter`, пока не попросят принять лицензию — введи `yes`. Затем соглашайся с установкой по умолчанию.

```bash
# 3. Перезапусти bash:
source ~/.bashrc

# 4. Проверь:
conda --version
```

Если видишь ошибку `conda: command not found` — добавь `conda` в PATH:

```bash
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

---

## 🎮 Поддержка GPU в WSL

Для `IsaacGym` требуется **доступ к GPU через CUDA**.

### ✅ У тебя есть NVIDIA GPU:

1. Установи [официальные драйверы NVIDIA для Windows](https://www.nvidia.com/Download/index.aspx)
2. Установи [CUDA для WSL](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)
3. Проверь:
   ```bash
   nvidia-smi
   ```

### ❌ Нет GPU? Есть варианты:

- ☁️ Запустить обучение на удалённом GPU:
  - Google Cloud / AWS / Paperspace / LambdaLabs
- 🧪 Альтернатива: попробовать портировать на Google Colab

---

## 📦 Шаг 2: Скачиваем модель робота (будет позже)


---

## 🧪 Шаг 3: Создаём и активируем окружение Conda

```bash
conda create -n Tinker python=3.8.10
conda activate Tinker
```

Если не получилось с первого раза, попробуйте запустить
```bash
conda init
source ~/.bashrc
conda activate Tinker
```

Учтите, что если вы увидите активированное окружение (base), нужно выйти из него и активировать Tinker
```bash
conda deactivate
conda activate Tinker
```

---

## 📥 Шаг 4: Установка зависимостей

Рекомендуется устанавливать зависимые пакеты **по группам**, чтобы легче было отлавливать ошибки.

```bash
pip install torchvision -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install torch -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install pyquaternion -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install pyyaml -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install rospkg -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install pexpect -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install mujoco==2.3.7 -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install mujoco-py -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install mujoco-python-viewer -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install dm_control==1.0.14 -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install einops -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install tqdm -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install packaging -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install h5py -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install ipython -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install getkey -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install wandb -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install chardet -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install numpy==1.23.2 -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install h5py_cache -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install tensorboard -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install lcm -i https://pypi.tuna.tsinghua.edu.cn/simple
```

Потребуется доустановить две библиотеки для установки пакета `generate-parameter-library-py 0.4.0`
```bash
pip install pyyaml typeguard
```
А затем повторить
```bash
pip install generate-parameter-library-py
```


Проверить сколько места занимают установленные зависимости можно командой
```bash
du -sh ~/miniconda3/envs/Tinker
```
У меня получилось 5.9G

---

## 🤖 Шаг 5: Установка IsaacGym

1. Перейди на сайт NVIDIA и скачай IsaacGym (регистрация нужна)
2. Распакуй в удобную папку, например:
   ```bash
   mkdir -p ~/tools && cd ~/tools
   unzip ~/Загрузки/IsaacGym_Preview_3.zip -d IsaacGym
   ```
3. Установи Python-модуль:
   ```bash
   cd ~/tools/IsaacGym/python
   pip install -e .
   ```

📌 **Важно**: IsaacGym работает только с **GPU и CUDA**. Без них не получится тренировать модель.

---

## 🔧 Шаг 6: Просмотр URDF модели

Если хочешь визуализировать URDF в RViz, выполни:

```bash
roslaunch urdf_tutorial display.launch model:=/путь/к/tinker_urdf.urdf
```

Например:

```bash
roslaunch urdf_tutorial display.launch model:=/home/bitbridge/Tinker/resources/tinker/urdf/tinker_urdf.urdf
```

📌 Убедись, что пакет `urdf_tutorial` установлен:  
```bash
sudo apt install ros-${ROS_DISTRO}-urdf-tutorial
```

---


