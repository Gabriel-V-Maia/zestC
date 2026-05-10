```markdown
# zestC
ZestC é uma framework de testes em C, feita em apenas um arquivo.
Ela foca ser simples, e no futuro, mais customizada, fiz ela para uso próprio mas acredito que outros devem achar utilidade.

# Implementação

```
projeto/
├── zest.h
├── main.c
└── tests/
    ├── test_math.c
    └── test_string.c
```

Cada arquivo de teste inclui `zest.h` e define seus testes com `ZEST_TEST`:

```c
// tests/test_math.c
#include "../zest.h"

ZEST_TEST(test_soma) {
    ZEST_ASSERT_EQ(2 + 2, 4);
}

ZEST_TEST(test_subtracao) {
    ZEST_ASSERT_EQ(10 - 3, 7);
}
```

```c
// tests/test_string.c
#include "../zest.h"

ZEST_TEST(test_str_eq) {
    ZEST_ASSERT_STR_EQ("zest", "zest");
}

ZEST_TEST(test_str_notnull) {
    const char *s = "hello";
    ZEST_ASSERT_NOTNULL(s);
}
```

No `main.c`, defina `ZEST_IMPLEMENTATION` antes de incluir o header, inclua os arquivos de teste e registre tudo com `ZEST_RUN`:

```c
// main.c
#define ZEST_IMPLEMENTATION
#include "zest.h"

#include "tests/test_math.c"
#include "tests/test_string.c"

int main(void) {
    ZEST_RUN("tests/", 1, "tests.log",
        ZEST_ENTRY(test_soma),
        ZEST_ENTRY(test_subtracao),
        ZEST_ENTRY(test_str_eq),
        ZEST_ENTRY(test_str_notnull)
    );
    return 0;
}
```

Compile e rode com:

```bash
gcc main.c -o tests && ./tests
```

# Macros disponíveis

| Macro | Descrição |
|---|---|
| `ZEST_TEST(name)` | Define um teste |
| `ZEST_ENTRY(name)` | Registra um teste no runner |
| `ZEST_ASSERT(expr)` | Falha se expressão for falsa |
| `ZEST_ASSERT_EQ(a, b)` | Falha se `a != b` |
| `ZEST_ASSERT_NEQ(a, b)` | Falha se `a == b` |
| `ZEST_ASSERT_NULL(p)` | Falha se `p != NULL` |
| `ZEST_ASSERT_NOTNULL(p)` | Falha se `p == NULL` |
| `ZEST_ASSERT_STR_EQ(a, b)` | Falha se strings forem diferentes |
```