# КрестикиНоликиОнлайн

Разработать Rest сервис для игры в крестики нолики.

Игроки по очереди ставят на свободные клетки поля 3×3 знаки (один всегда крестики, другой всегда нолики).
Первый, выстроивший в ряд 3 своих фигуры по вертикали, горизонтали или большой диагонали, выигрывает.
Если игроки заполнили все 9 ячеек и оказалось, что ни в одной вертикали, горизонтали или большой диагонали нет трёх одинаковых знаков, партия считается закончившейся в ничью.
Первый ход делает игрок, ставящий крестики. 

Состояние поля хранится в БД.

Предусмотреть возможность масштабирования сервиса и конкурентного доступа к партии (одновременно пытаются сделать ход оба игрока).

Опционально разработать web интерфейс, игра на поле произвольной размерности.

## API сервиса

### Выполнения хода

`POST /v1/{UUID}/{x|o}`
```json
{
    "xc":"1",
    "yc":"1"
}
```

- `UUID` - Id партии
- `x|o` - за кого выполняется ход  (или x, или o)
- `xc` - горизонтальная координата, целое число
- `yc` - вертикальная координата, целое число

Ответ
```json
{
    "winner":"x|o|draw|null"
}
```

- `winner` - результат партии. "x" - выиграли крестики, "o" - выиграли нолики, "draw" - ничья. Если партия не завершилась - null

Коды ошибок

- 403 - координаты клетки выходит за границы поля
- 409 - клетка занята
- 422 - нарушена последовательность ходов

Тело ошибки
```json
{
    "message": "Не твой ход"
}
```
