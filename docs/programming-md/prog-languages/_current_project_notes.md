# Trelo kéo thả

## Intro

pages/Boards/ gọi là board detail. Ở trong có AppBar, BoardBar, BoardContent.

`BoardContent` có chứa `ListColumns`. Trong đó lại chứa Column > ListCards > Card

AppBar nằm trong components vì nó được dùng lại ở nhiều screen.

BoardContent `<DndContext>`
ListColumns `<SortableContext>`
