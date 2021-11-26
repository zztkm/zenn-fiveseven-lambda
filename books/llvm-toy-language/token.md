---
title: "トークン"
---

字句解析のために，まずはトークンを定義する．

# Token
まずは基底クラスから作る．

```cpp:token.hpp
#include <memory>
#include <utility>

#include "pos.hpp"

namespace token {
    class Token {
    public:
        virtual ~Token();
        virtual void debug_print() = 0;
    };
    using TokenWithPos = std::pair<pos::Range, std::unique_ptr<Token>>;
}
```
```cpp:token.cpp
#include "token.hpp"

namespace token {
    Token::~Token() = default;
}
```
`debug_print()` はデバッグ出力用の関数だ．いずれ消す．

トークンは複数文字にわたるので，位置を表すのに `pos::Range` を使う．たぶんけっこうな頻度で `pos::Range` と一緒に使うので，`TokenWithPos` も定義しておく．`Token` に `pos::Range` をメンバとして持たせない理由は，後で字句解析の様子を見ると分かる．
# Identifier
識別子は，正規表現 `[A-Za-z_][A-Za-z_0-9]*` に合致するものとする．

```cpp:token.hpp
#include <string>

namespace token {
    class Identifier : public Token {
        std::string name;
    public:
        Identifier(std::string &&);
        void debug_print() override;
    };
}
```
```cpp:token.cpp
namespace token {
    Identifier::Identifier(std::string &&name): name(std::move(name)) {}
    void Identifier::debug_print(){
        std::cout << "Identifier(" << name << ")" << std::endl;
    }
}
```
# Integer
整数リテラルは `[0-9]+`．

ゆくゆくは `0b` `0o` `0x` で始まる 2，8，16 進リテラルや浮動小数点数リテラルも考えたいけど，今はやめておく．

整数なのにメンバ `value` の型は `std::string`．あとでこれを 32 ビット整数値に直すとき `2147483648` 以上はエラーとするけれど，前に `-` が付いた `-2147483648` は ok とする．
```cpp:token.hpp
namespace token {
    class Integer : public Token {
        std::string value;
    public:
        Integer(std::string &&);
        void debug_print() override;
    };
}
```
```cpp:token.cpp
namespace token {
    Integer::Integer(std::string &&value): value(std::move(value)) {}
    void Integer::debug_print(){
        std::cout << "Integer(" << value << ")" << std::endl;
    }
}
```
# 記号類
何があるといい？
- 四則演算 `+` `-` `*` `/` `%`
- ビット演算 `&` `|` `^` `~` `<<` `>>`
- 代入 `=` `+=` `-=` `*=` `/=` `%=` `&=` `|=` `^=` `<<=` `>>=`
- 論理演算 `&&` `||` `!`
- 比較 `==` `!=` `<` `>` `<=` `>=`
- 括弧 `(` `)` `{` `}` `[` `]`
- 区切り文字 `.` `:` `;` `,`
```cpp:token.hpp
namespace token {
    class Plus : public Token {
        void debug_print() override;
    };
    class Hyphen : public Token {
        void debug_print() override;
    };
    /* 以下略 */
}
```
```cpp:token.cpp
#define define_debug_print(token) \
    void token::debug_print(){ \
        std::cout << #token << std::endl; \
    }

namespace token {
    define_debug_print(Plus)
    define_debug_print(Hyphen)
    /* 略 */
}
```
いずれここにたくさん純粋仮想関数を追加する．


使える文字の中ではあと `#` と `?` が残ってる．