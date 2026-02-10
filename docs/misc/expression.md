author: Ir1d, Anguei, hsfzLZH1, siger-young, HeRaNO, c8ef

Đánh giá biểu thức là bài toán: nhập một chuỗi biểu thức và xuất giá trị của nó. Có nhiều biến thể: có/không có ngoặc, có lũy thừa, bao nhiêu biến, kiểm tra hai biểu thức tương đương, v.v.

Biểu thức thường cần phân tích cú pháp (grammar parsing) rồi mới tính, hoặc vừa phân tích vừa tính. Phân tích cú pháp dùng để kiểm tra chuỗi có là biểu thức hợp lệ không, thường dùng parser.

Biểu thức gồm hai loại ký tự: toán hạng và toán tử. Với biểu thức độ dài $n$, dùng phương pháp phân tích phù hợp có thể xử lý trong $O(n)$.

## Cây biểu thức và biểu thức Ba Lan ngược

Một cách phân tích đệ quy là coi biểu thức theo luật cú pháp, xây cây biểu thức như hình rồi tính từ dưới lên. ![](./images/bet.png)

Duyệt cây biểu thức sẽ tạo ra các dạng biểu thức khác nhau. Có ba dạng: tiền tố, trung tố, hậu tố. Trung tố là dạng thường dùng; hậu tố là dạng máy tính dễ xử lý.

-   Duyệt tiền tự: biểu thức tiền tố (Ba Lan)
-   Duyệt trung tự: biểu thức trung tố
-   Duyệt hậu tự: biểu thức hậu tố (Ba Lan ngược)

Biểu thức Ba Lan ngược (hậu tố) là dạng trong đó toán tử đứng sau toán hạng. Ví dụ:

$$
a+b*c*d+(e-f)*(g*h+i)
$$

có thể viết dưới dạng hậu tố:

$$
abc*d*+ef-gh*i+*+
$$

Do đó, biểu thức hậu tố tương ứng 1–1 với cây biểu thức. Nó không cần ngoặc, thứ tự tính toán là duy nhất.

Ưu điểm: tính rất dễ trong thời gian tuyến tính. Ví dụ với $3~2~*~1~-$: trước hết $3\times2=6$ (dùng toán tử cuối cùng, tức toán tử trên đỉnh stack), rồi $6-1=5$. Tức là chỉ cần **duy trì một stack số, gặp toán tử thì lấy hai phần tử trên cùng, tính rồi đẩy lại**. Phần tử cuối cùng là kết quả. Thuật toán chạy $O(n)$.

Phân tích đệ quy phụ thuộc vào thiết kế ngữ pháp, tức có xây đúng cây biểu thức mong muốn hay không. Ví dụ:

$$
a+b*c
$$

Do độ ưu tiên khác nhau giữa cộng và nhân, biểu thức trung tố có thể cho hai cây khác nhau. Do đó, thiết kế ngữ pháp phụ thuộc mạnh vào độ ưu tiên, và không dễ.

Phần sau coi toán tử và độ ưu tiên là một khối, dùng phương pháp không đệ quy, trực tiếp dựa trên độ ưu tiên để phân tích và tính.

## Biểu thức có ngoặc chỉ gồm toán tử nhị phân kết hợp trái

Xét bài toán đơn giản: mọi toán tử là nhị phân và kết hợp trái (nếu cùng độ ưu tiên thì tính từ trái sang phải). Cho phép dùng ngoặc.

Với loại trung tố này, có thể chuyển sang hậu tố rồi tính. Dùng hai [stack](../ds/stack.md) để lưu toán tử và toán hạng. Gặp số thì đẩy vào stack số. Mỗi “khối toán tử” tương ứng một cặp ngoặc, stack toán tử chỉ cần đơn điệu trong khối. Gặp toán tử thì lấy khối trên cùng, bật các toán tử có độ ưu tiên không nhỏ hơn và đồng thời tính toán.

Trong phần sau, “xuất” nghĩa là đưa ra biểu thức hậu tố, tức đẩy số vào stack số, hoặc bật toán tử cùng hai toán hạng, tính rồi đẩy kết quả. Quét từ trái sang phải:

1.  Nếu gặp số, xuất số.
2.  Nếu gặp ngoặc trái, đẩy vào stack toán tử.
3.  Nếu gặp ngoặc phải, liên tục xuất đỉnh cho tới khi gặp ngoặc trái, rồi bật ngoặc trái. Tức là tính toàn bộ trong ngoặc.
4.  Nếu gặp toán tử khác, liên tục xuất mọi toán tử có độ ưu tiên $\ge$ toán tử hiện tại. Cuối cùng đẩy toán tử hiện tại.
5.  Quét xong, xuất nốt các toán tử còn lại trong stack.

Cài đặt cho bốn toán tử $+$、$-$、$*$、$/$:

??? note "示例代码"
    ```cpp
    
    bool delim(char c) { return c == ' '; }
    
    bool is_op(char c) { return c == '+' || c == '-' || c == '*' || c == '/'; }
    
    int priority(char op) {
      if (op == '+' || op == '-') return 1;
      if (op == '*' || op == '/') return 2;
      return -1;
    }
    
    void process_op(stack<int>& st, char op) {  // Cũng có thể dùng để tính biểu thức hậu tố
      int r = st.top();                         // Lấy phần tử đỉnh, chú ý thứ tự
      st.pop();
      int l = st.top();
      st.pop();
      switch (op) {
        case '+':
          st.push(l + r);
          break;
        case '-':
          st.push(l - r);
          break;
        case '*':
          st.push(l * r);
          break;
        case '/':
          st.push(l / r);
          break;
      }
    }
    
    int evaluate(string& s) {  // Cũng có thể chỉnh để chuyển trung tố -> hậu tố
      stack<int> st;
      stack<char> op;
      for (int i = 0; i < (int)s.size(); i++) {
        if (delim(s[i])) continue;
    
        if (s[i] == '(') {
          op.push('(');  // 2. Gặp ngoặc trái thì đẩy vào stack toán tử
        } else if (s[i] == ')') {  // 3. Gặp ngoặc phải, thực hiện mọi phép trong ngoặc
          while (op.top() != '(') {
            process_op(st, op.top());
            op.pop();  // Liên tục xuất đỉnh cho tới ngoặc trái
          }
          op.pop();                // Bật ngoặc trái
        } else if (is_op(s[i])) {  // 4. Gặp toán tử khác
          char cur_op = s[i];
          while (!op.empty() && priority(op.top()) >= priority(cur_op)) {
            process_op(st, op.top());
            op.pop();  // Xuất mọi toán tử có độ ưu tiên >= hiện tại
          }
          op.push(cur_op);  // Đẩy toán tử mới
        } else {            // 1. Gặp số, xuất số
          int number = 0;
          while (i < (int)s.size() && isalnum(s[i]))
            number = number * 10 + s[i++] - '0';
          --i;
          st.push(number);
        }
      }
    
      while (!op.empty()) {
        process_op(st, op.top());
        op.pop();
      }
      return st.top();
    }
    
    ```

Thuật toán này chạy $O(n)$. Có thể chỉnh để xuất biểu thức hậu tố tường minh.

### Toán tử một ngôi và toán tử kết hợp phải

Giả sử biểu thức có toán tử một ngôi (một toán hạng), ví dụ dấu cộng/trừ một ngôi.

Cần xác định toán tử hiện tại là một ngôi hay nhị phân.

Lưu ý, trước toán tử một ngôi thường là một toán tử khác hoặc ngoặc trái; nếu ở đầu biểu thức thì không có. Trước toán tử nhị phân luôn là toán hạng hoặc ngoặc phải. Vì vậy có thể đánh dấu xem toán tử kế tiếp có thể là một ngôi hay không.

Ngoài ra, cần xử lý khác nhau giữa một ngôi và nhị phân, đồng thời cho một ngôi có độ ưu tiên cao hơn. Một số toán tử một ngôi (như +, -) thực chất là kết hợp phải.

Kết hợp phải nghĩa là nếu cùng độ ưu tiên thì tính từ phải sang trái.

Ví dụ khác của kết hợp phải là lũy thừa. Với $a \wedge b \wedge c$, thường hiểu là $a^{b^c}$ chứ không phải $(a^b)^c$.

Để xử lý đúng, khi độ ưu tiên bằng nhau, ta trì hoãn việc bật toán tử.

Cần sửa đoạn:

```cpp

while (!op.empty() && priority(op.top()) >= priority(cur_op)) 
```

thành:

```cpp

while (!op.empty() &&
       ((left_assoc(cur_op) && priority(op.top()) >= priority(cur_op)) ||
        (!left_assoc(cur_op) && priority(op.top()) > priority(cur_op))))

```

Trong đó left\_assoc quyết định toán tử có kết hợp trái hay không.

Dưới đây là cài đặt cho $+$、$-$、$*$、$/$ nhị phân và $+$、$-$ một ngôi:

??? note "示例代码"
    ```cpp
    
    bool delim(char c) { return c == ' '; }
    
    bool is_op(char c) { return c == '+' || c == '-' || c == '*' || c == '/'; }
    
    bool is_unary(char c) { return c == '+' || c == '-'; }
    
    int priority(char op) {
      if (op < 0)  // Toán tử một ngôi
        return 3;
      if (op == '+' || op == '-') return 1;
      if (op == '*' || op == '/') return 2;
      return -1;
    }
    
    void process_op(stack<int>& st, char op) {
      if (op < 0) {
        int l = st.top();
        st.pop();
        switch (-op) {
          case '+':
            st.push(l);
            break;
          case '-':
            st.push(-l);
            break;
        }
      } else {  // Lấy phần tử đỉnh, chú ý thứ tự
        int r = st.top();
        st.pop();
        int l = st.top();
        st.pop();
        switch (op) {
          case '+':
            st.push(l + r);
            break;
          case '-':
            st.push(l - r);
            break;
          case '*':
            st.push(l * r);
            break;
          case '/':
            st.push(l / r);
            break;
        }
      }
    }
    
    int evaluate(string& s) {
      stack<int> st;
      stack<char> op;
      bool may_be_unary = true;
      for (int i = 0; i < (int)s.size(); i++) {
        if (delim(s[i])) continue;
    
        if (s[i] == '(') {
          op.push('(');  // 2. Gặp ngoặc trái thì đẩy vào stack toán tử
          may_be_unary = true;
        } else if (s[i] == ')') {  // 3. Gặp ngoặc phải, thực hiện mọi phép trong ngoặc
          while (op.top() != '(') {
            process_op(st, op.top());
            op.pop();  // Liên tục xuất đỉnh cho tới ngoặc trái
          }
          op.pop();  // Bật ngoặc trái
          may_be_unary = false;
        } else if (is_op(s[i])) {  // 4. Gặp toán tử khác
          char cur_op = s[i];
          if (may_be_unary && is_unary(cur_op)) cur_op = -cur_op;
          while (!op.empty() &&
                 ((cur_op >= 0 && priority(op.top()) >= priority(cur_op)) ||
                  (cur_op < 0 && priority(op.top()) > priority(cur_op)))) {
            process_op(st, op.top());
            op.pop();  // Xuất mọi toán tử có độ ưu tiên >= hiện tại
          }
          op.push(cur_op);  // Đẩy toán tử mới
          may_be_unary = true;
        } else {  // 1. Gặp số, xuất số
          int number = 0;
          while (i < (int)s.size() && isalnum(s[i]))
            number = number * 10 + s[i++] - '0';
          --i;
          st.push(number);
          may_be_unary = false;
        }
      }
    
      while (!op.empty()) {
        process_op(st, op.top());
        op.pop();
      }
      return st.top();
    }
    
    ```

## Tài liệu tham khảo

**Trang này chủ yếu dịch từ [Разбор выражений. Обратная польская нотация](https://e-maxx.ru/algo/expressions_parsing) và bản dịch tiếng Anh [Expression parsing](https://cp-algorithms.com/string/expression_parsing.html). Bản tiếng Nga theo Public Domain + Leave a Link; bản tiếng Anh theo CC-BY-SA 4.0.**

## Đọc thêm

1.  [Operator-precedence_parser](https://en.wikipedia.org/wiki/Operator-precedence_parser)
2.  [Shunting yard algorithm](https://en.wikipedia.org/wiki/Shunting_yard_algorithm)

## Bài tập

1.  [NOIP2013 普及组 表达式求值](https://www.luogu.com.cn/problem/P1981)
2.  [后缀表达式](https://www.luogu.com.cn/problem/P1449)
3.  [Transform the Expression](https://www.spoj.com/problems/ONP/)
