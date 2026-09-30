A ideia é simples e consiste em
> reservar um bloco grande de memória de uma única vez.
> E, a partir dele, distribuir blocos conforme necessario

Criar uma estrutura

```c
void *memory;
size_t capacity;
size_t offset.
```
para a arena

Implemente

```c
bool arena_init(Arena *arena, size_t capacity);
void *arena_alloc(Arena *arena, size_t size);
void arena_reset(Arena *arena);
void arena_destroy(Arena *arena);
```

o comportamento esperado 
```c
Arena arena;

arena_init(&arena, 1024);

int *a = arena_alloc(&arena, sizeof(int));
char *b = arena_alloc(&arena, 32);
```

Cada `arena_alloc()` simplesmente pega a memória a partir do **offset** atual e avança esse offset

por exemplo

```m
capacity = 1024
offset   = 0

alloc(4)
  -  retorna memory + 0
  -  offset = 0

alloc(32)
  -  retorna memory + 4
  -  offset = 36

alloc(100)
  -  retorna memory + 36
  -  offset = 136
```

regrinhas

1. `arena_init()` deve fazer uma **única** **alocação** para o bloco principal.
2. `arena_alloc()` **não** pode chamar `malloc()`
3. Se não houver espaço suficiente, arena_alloc **DEVE** retornar null.
4. `arena_reset()` **deve** tornar **todo** o espaço novamente **disponivel**, **sem** precisar **alocar** memoria nova.
5. `arena_destroy()` deve liberar o bloco principal e deixar em estado seguro para não ser destruida duas vezes.
6. Não é necessario implementar `free()` individual.
7.  não usar `realloc()`, `calloc()`
8. `arena_alloc()` **deve** trabalhar com aritmética de ponteiros.
9. evite casts desnecessarios.
10. faça tratamento de overflow (offset + size).
