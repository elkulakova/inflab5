# Лабораторная работа №5

## Задание 1

1) Так как каждый репозиторий на `GitHub` создается сразу с папкой `.git`, в которой лежит папка `hooks`, необходимая нам, то создадим в ней файл `pre-commit`:

```
touch .git/hookers/pre-commit
```
Именно этот файл будет служить нам в качестве хука. Также для его исполняемости выдадим права всем пользователям:

```
chmod +x .git/hookers/pre-commit
```

2) Для того, чтобы при коммите обрабатывались файлы формата `.txt`, напишем следующую функцию:

```
error_found=0

for file in $(git diff --cached --name-only --diff-filter=ACMR | grep "\.txt$"); do
    if ! grep '[^[:space:]]' "$file"; then
        echo "error: '$file' isn't supposed to be empty."
    	error_found=1    
    fi
done

if [ "$error_found" -eq 1 ]; then
	exit 1
fi

exit 0
```
Весь файл `pre-commit` выглядит следующим образом:

<p align="center">
 <img width="600px" src="code.png" alt="qr"/>
</p>

3) Теперь для проверки работоспособности хука создадим два файла:
   - `empty.txt` пустой файл
   - `trial.txt` файл, заполненный пробелами, переносами строки и табуляциями
Попробуем закоммитить эти файлы:

```
git add empty.txt trial.txt
git commit -m "trying to commit empty files"
```

И увидим в терминале следующее:

<p align="center">
 <img width="600px" src="hooked.png" alt="qr"/>
</p>
