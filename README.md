# Домашнее задание к лабораторной работе №10
## Индивидуальное задание. Фигура: остроугольный треугольник (вариант 8)
Программа предлагает меню:
1. рассчитать площадь;
2. вывести определение фигуры;
3. нарисовать фигуру;
4. сравнить два треугольника по площади;
0. выход.

Фигура — закрашенный равнобедренный остроугольный треугольник: основание `s` и равные углы `a` при основании (45° < `a` < 90°, тогда угол при вершине меньше 90°). Площадь: S = s² · tg(a) / 4.

Для каждой операции написана отдельная функция:
- `double area_triangle(double s, double a)` — площадь;
- `void print_definition(void)` — определение фигуры;
- `void draw_triangle(char sym, int s, double alpha)` — рисование символом `sym`, вид символа и размеры задаются в `main`;
- `int get_side(void)` и `double get_angle(void)` — ввод и возврат параметров фигуры (сторона и угол);
- `double compare_triangle(double s1, double a1, double s2, double a2)` — разница площадей двух треугольников (первый минус второй).

Меню повторяется циклом `do-while`, пока пользователь не выберет пункт 0.
### Алгоритм

1. Начало.
2. Повторять (цикл `do-while`), пока `choice != 0`:
   - вывести меню и ввести `choice`;
   - `choice = 1` — ввести `s` и `a` (функции `get_side`, `get_angle`), вывести `area_triangle(s, a)`;
   - `choice = 2` — вывести определение фигуры (`print_definition`);
   - `choice = 3` — ввести символ `sym`, `s` и `a`, нарисовать фигуру (`draw_triangle`);
   - `choice = 4` — ввести `s`, `a` для двух треугольников, вычислить `diff = compare_triangle(...)` и вывести, какая площадь больше;
   - `choice = 0` — вывести «Выход»;
   - иначе — вывести сообщение об ошибке.
3. Конец.

### Блок-схема
<img width="1239" height="769" alt="изображение" src="https://github.com/user-attachments/assets/48e25a60-1555-4539-8142-f77e245d5aca" />

### Результат
```
Фигура: остроугольный треугольник (заполненный)

Меню:
 1 - рассчитать площадь
 2 - вывести определение фигуры
 3 - нарисовать фигуру
 4 - сравнить два треугольника по площади
 0 - выход
Выберите пункт: 1
Введите сторону s (2..80): 20
Введите угол a в градусах (больше 45 и меньше 90): 60
Площадь треугольника: 173.21

Выберите пункт: 2
Остроугольный треугольник - это треугольник, у которого все три угла острые (меньше 90 градусов).
В программе строится равнобедренный остроугольный треугольник:
основание s и равные углы a при основании, 45 < a < 90.
Площадь: S = s^2 * tg(a) / 4

Выберите пункт: 3
Введите символ, из которого будет строиться фигура: *
Введите сторону s (2..80): 20
Введите угол a в градусах (больше 45 и меньше 90): 60

         **
        ****
       ******
      ********
     **********
   **************
  ****************
 ******************
********************

Выберите пункт: 4
Первый треугольник:
Введите сторону s (2..80): 10
Введите угол a в градусах (больше 45 и меньше 90): 60
Второй треугольник:
Введите сторону s (2..80): 12
Введите угол a в градусах (больше 45 и меньше 90): 60
Площадь второго больше на 19.05

Выберите пункт: 7
Ошибка: такого пункта нет

Выберите пункт: 0
Выход
```
## Код программы
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <locale.h>

#define PI 3.14159265358979
#define ASPECT 2.0
#define MAX_ROWS 60

double area_triangle(double s, double a);
void print_definition(void);
void draw_triangle(char sym, int s, double alpha);
int get_side(void);
double get_angle(void);
double compare_triangle(double s1, double a1, double s2, double a2);

int main(void)
{
    int choice;
    int s, s2;
    double alpha, alpha2;
    char sym;
    double diff;

    setlocale(LC_ALL, ".UTF8");

    printf("Фигура: остроугольный треугольник (заполненный)\n");
    do
    {
        printf("\nМеню:\n");
        printf(" 1 - рассчитать площадь\n");
        printf(" 2 - вывести определение фигуры\n");
        printf(" 3 - нарисовать фигуру\n");
        printf(" 4 - сравнить два треугольника по площади\n");
        printf(" 0 - выход\n");
        printf("Выберите пункт: ");
        if (scanf("%d", &choice) != 1)
        {
            printf("Ошибка: нужно ввести номер пункта\n");
            return 1;
        }

        switch (choice)
        {
        case 1:
            s = get_side();
            alpha = get_angle();
            printf("Площадь треугольника: %.2f\n", area_triangle(s, alpha));
            break;
        case 2:
            print_definition();
            break;
        case 3:
            printf("Введите символ, из которого будет строиться фигура: ");
            scanf(" %c", &sym);
            s = get_side();
            alpha = get_angle();
            draw_triangle(sym, s, alpha);
            break;
        case 4:
            printf("Первый треугольник:\n");
            s = get_side();
            alpha = get_angle();
            printf("Второй треугольник:\n");
            s2 = get_side();
            alpha2 = get_angle();
            diff = compare_triangle(s, alpha, s2, alpha2);
            if (diff > 0)
                printf("Площадь первого больше на %.2f\n", diff);
            else if (diff < 0)
                printf("Площадь второго больше на %.2f\n", -diff);
            else
                printf("Площади равны\n");
            break;
        case 0:
            printf("Выход\n");
            break;
        default:
            printf("Ошибка: такого пункта нет\n");
        }
    } while (choice != 0);

    return 0;
}

double area_triangle(double s, double a)
{
    double area;
    area = s * s / 4 * tan(a * PI / 180.0);
    return area;
}

void print_definition(void)
{
    printf("Остроугольный треугольник - это треугольник, у которого все три угла острые (меньше 90 градусов).\n");
    printf("В программе строится равнобедренный остроугольный треугольник:\n");
    printf("основание s и равные углы a при основании, 45 < a < 90.\n");
    printf("Площадь: S = s^2 * tg(a) / 4\n");
}

void draw_triangle(char sym, int s, double alpha)
{
    int r, c, rows;
    double tg, height, y, inset, x;

    tg = tan(alpha * PI / 180.0);
    height = s / 2.0 * tg;
    rows = (int)ceil(height / ASPECT);
    if (rows < 1)
        rows = 1;

    if (rows > MAX_ROWS)
    {
        printf("Ошибка: фигура слишком высокая, уменьшите сторону или угол\n");
        return;
    }

    printf("\n");
    for (r = rows - 1; r >= 0; r--)
    {
        y = r * ASPECT;
        inset = y / tg;
        for (c = 0; c < s; c++)
        {
            x = c + 0.5;
            if (x >= inset && x <= s - inset)
                putchar(sym);
            else
                putchar(' ');
        }
        putchar('\n');
    }
}

int get_side(void)
{
    int s;
    int ch;

    do
    {
        printf("Введите сторону s (2..80): ");
        if (scanf("%d", &s) != 1)
        {
            while ((ch = getchar()) != '\n' && ch != EOF)
                ;
            if (ch == EOF)
                exit(1);
            s = 0;
        }
    } while (s < 2 || s > 80);

    return s;
}

/* Возвращает угол a при основании, введённый пользователем (45 < a < 90) */
double get_angle(void)
{
    double a;
    int ch;

    do
    {
        printf("Введите угол a в градусах (больше 45 и меньше 90): ");
        if (scanf("%lf", &a) != 1)
        {
            while ((ch = getchar()) != '\n' && ch != EOF)
                ;
            if (ch == EOF)
                exit(1);
            a = 0;
        }
    } while (a <= 45 || a >= 90);

    return a;
}

/* Разница площадей двух треугольников (первый минус второй) */
double compare_triangle(double s1, double a1, double s2, double a2)
{
    double diff;
    diff = area_triangle(s1, a1) - area_triangle(s2, a2);
    return diff;
}
```
## Выполнил
Студент Еркимбеков Дмитрий бИД-261
