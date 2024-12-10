# 栈与队列（Stack and Queue）

# 栈

堆栈（stack）是一种数据结构，元素的进入遵循先进后出（FILO  First In Last Out）原则。我们把堆叠元素的顶部称为“栈顶”，底部称为“栈底”。将把元素添加到栈顶的操作叫做“入栈”，删除栈顶元素的操作叫做“出栈”。由于先进后出的限制，我们只能在栈顶添加或者删除元素。**因此栈可以被视为一种受限制的数据结构，受限制的数组或链表**。也就是说我们可以屏蔽它们的部分无关操作达到受限的效果。



----





### 1.基于链表的实现

- 代码实现

  这是一个简单的链表栈的定义部分 构造函数以及析构函数； 基于链表实现栈时，我们可以将链表的头节点视为栈顶，尾节点视为栈底。

```c
typedef struct ListNode{
    struct ListNode *next;
    int val;
    
}ListNode;

typedef struct {
    ListNode *top;
    int size;
}Stack;

/*构造函数*/
Stack *newLinkedlistStack(){
    Stack *s = malloc(sizeof(Stack));
    s->top = NULL;
    s->size = 0;
    return s;
}

/*析构函数*/
Stack *delLinkedlistStack(Stack *s){
    while(s->top){
        ListNode *n = s->top->next;
        free(s->top);
        s-top = n
    }
	free(s);
}

int size(Stack *s){
    return s->size;
}

bool isEmpty(Stack *s){
    return size(s) == 0;
}
```



以下部分是主要的入栈（push）访问栈顶元素（peek） 出栈（pop）操作 

 链栈入栈主要为**头插法建立链表**

访问栈顶元素需要先判断栈是否为空  然后**直接返回栈顶元素值**

出栈：

1.先访问栈顶元素

2.引入一个tmp变量用来释放掉原栈顶的空间

3.容量缩减

```c

/*入栈*/
void push(Stack *s ,int num){
	ListNode *node = (ListNode *) malloc (sizeof(ListNode));//动态申请空间
    node->next = s->top;
    node->val = num;
    s->top = node;
    s->size++;
}


/*访问栈顶*/
int peek(Stack *s){
    if(s->size == 0){
        printf("栈为空\n");
        return INT_MAX;     
    }
	return s->top->val;
}

/*出栈*/
void pop(Stack *s){
    //访问栈顶元素
    int val = peek(s);
    
    //释放空间
	ListNode *tmp = s->top; 
    s->top = s->top->next;
    free(tmp);
    
    //缩减容量
    s->size--;
	return val;
}

```



### 2.基于数组的实现

下面是一个数组顺序栈的基本功能实现代码：

由于数组的长度是固定的 所以需要加入扩容机制

push只需要stack->data[(stack->size++)] = num;

```C
#define MAX_SIZE 100      // 初始大小
#define GROWTH_FACTOR 2 

typedef int elemType;
typedef struct{
    elemType *data;
    int size;
	int capacity;
}ArrayStack;

//构造函数
ArrayStack* NewArrayStack() {
    ArrayStack* stack = (ArrayStack*)malloc(sizeof(ArrayStack));
    // 初始化数组
    stack->data = (elemType*)malloc(sizeof(elemType) * MAX_SIZE) 
    stack->size = 0;
    stack->capacity = MAX_SIZE;
    return stack;
}

//扩容函数
bool resize(ArrayStack* stack) {
    if (stack == NULL) return false;
    
    // 计算新容量
    int newCapacity = stack->capacity * GROWTH_FACTOR;
    // 重新分配内存
    elemType* newData = (elemType*)realloc(stack->data, sizeof(elemType) * newCapacity);
    // 检查内存分配是否成功
    if (newData == NULL) {
        return false;
    }
    
    // 更新栈的数据
    stack->data = newData;
    stack->capacity = newCapacity;
    return true;
}

//析构函数
void delArrayStack(ArrayStack* stack){
    free(stack->data);
    free(stack->);
}

int size(ArrayStack* stack){
    return stack->size;
 
}

//出栈
void push(ArrayStack* stack, int num) {
    if (stack == NULL) {
        return;
    }
    
    // 检查是否需要扩容
    if (stack->size >= stack->capacity) {
        if (!resize(stack)) {
            printf("扩容失败，栈已满\n");
            return;
        }
        printf("扩容成功，新容量：%d\n", stack->capacity);
    }
    
    // 添加新元素
    stack->data[stack->size++] = num;
}

elemType peek(ArrayStack* stack){
    if(stack->size == 0 ){
        printf("栈为空\n");
    }
	Return INT_MAX;
	return stack->data[stack->size-1];
}

elemType pop(ArrayStack* stack){
    elemType val = peek(stack);
    stack->size--;
    return val;
}
```



### 一些题目：

#### （1）有效的括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

**示例 1：**

> **输入：**s = "()"
>
> **输出：**true

**示例 2：**

> **输入：**s = "()[]{}"
>
> **输出：**true

**示例 3：**

> **输入：**s = "(]"
>
> **输出：**false

**示例 4：**

> **输入：**s = "([])"
>
> **输出：**true

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

```c
/*解法一: 建立新栈 此方法空间复杂度为O(n)*/
char pairs(char a){
    if(a == '}') return '{';
    if(a == ')') return '(';
    if(a == ']') return '[';
    return 0;
    
}
bool isValid(char* s) {
    int len = strlen(s);
    if(len % 2 == 1){
        return false;
    }
    int stk[len+1], top = 0;
    for(int i=0; i<len; i++){
        char ch = pairs(s[i]);
        if(ch){
            if(top == 0 || stk[--top]!= ch){
                return false;
            }
            
        }else{
            stk[top++] = s[i];
        }
        
      
    }
    return top == 0;  
}
```

```c
/*解法二: 原地立栈 此方法空间复杂度为O(1)*/
bool isValid(char* s) {
    // 建立右括号到左括号的映射
    char mp[128] = {};  // 初始化所有值为0
    mp[')'] = '(';
    mp[']'] = '[';
    mp['}'] = '{';
    
    int top = 0;  // 栈顶指针
    int len = strlen(s);
    if(len % 2 == 1){
        return false;
    }
    // 遍历字符串
    for (int i = 0; s[i]; i++) {
        char c = s[i];
        
        if (mp[c] == 0) {  
            // 遇到左括号：入栈
            s[top++] = c;
        } else if (top == 0 || s[--top] != mp[c]) {  
            // 遇到右括号：
            // 1. 栈空 或 2. 栈顶不匹配
            return false;
        }
    }
    
    // 栈为空说明所有括号都匹配
    return top == 0;
}
```



#### （2）逆波兰表达式求值

给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

 

**示例 1：**

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

**示例 2：**

```
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

**示例 3：**

```
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 

**提示：**

- `1 <= tokens.length <= 104`
- `tokens[i]` 是一个算符（`"+"`、`"-"`、`"*"` 或 `"/"`），或是在范围 `[-200, 200]` 内的一个整数

 

**逆波兰表达式：**

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

- 平常使用的算式则是一种中缀表达式，如 `( 1 + 2 ) * ( 3 + 4 )` 。
- 该算式的逆波兰表达式写法为 `( ( 1 2 + ) ( 3 4 + ) * )` 。

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 `1 2 + 3 4 + * `也可以依据次序计算出正确结果。
- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中



```c
bool isNumber(char* token) {
    return strlen(token) > 1 || ('0' <= token[0] && token[0] <= '9');
}


int evalRPN(char** tokens, int tokensSize) {
    int stack[10000];  // 假设栈大小足够
    int top = 0;
    
    for (int i = 0; i < tokensSize; i++) {
        const char* token = tokens[i];
        
        // 如果是数字，直接入栈
        if (isNumber(token)) {
            stack[top++] = atoi(token);
            continue;
        }
        
        // 如果是运算符，取出两个数进行运算
        int b = stack[--top];  // 第二个操作数
        int a = stack[--top];  // 第一个操作数
        
        // 根据运算符进行计算
        switch (token[0]) {
            case '+': stack[top++] = a + b; break;
            case '-': stack[top++] = a - b; break;
            case '*': stack[top++] = a * b; break;
            case '/': stack[top++] = a / b; break;
        }
    }
    
    return stack[0];  // 最后栈中只剩下结果
}
```









# 队列

## 1.基于链表的实现

```c
typedef struct {
    Listnode *front, *rear;
    int queSize;
}Queue;

Queue *newQueue(){
    Queue *q = (Queue *)malloc(sizeof(Queue));
    q->front = NULL;
    q->rear = NULL;
    q->queSize = 0;
    return q;
}

void delQueue(Queue* q){
    while(queue->front != NULL){
        Linklist* tmp = queue->front;
        queue->front = queue->front->next;
        free(tmp);
    }
    free(q);
}

int Size(Queue *q){
    return q->queSize;
}

bool empty(Queue *queue){
    return(size(queue) == 0 );
}

void push(Queue *queue, int num){
    ListNode* node = newListNode(num);
    if(queue->front == NULL){
        queue->front = node;
        queue->rear = node;
    }
	else{
        queue->rear->next = node;
        queue->rear = node;
    }
}

int peek(Queue *q){
    return q->front->val;
}

int pop(Queue *q){
    int num = peek(q);
    Linklist *tmp = q->front;
    q->front = q->front->next;
    free(tmp);
    q->queSize--;
    return num;
}

```

## 2.基于环形数组的实现

```c
typedef struct{
    int *nums;
    int front;
    int rear;
    int Capacity;
}Queue;

Queue *newQueue(int capacity) {
    Queue *queue = (Queue*)malloc(sizeof(Queue));
    queue->capacity = capacity;
    queue->nums = (int *)malloc(sizeof(int) * queue->Capacity);
    queue->front = queue->rear = 0 ;
    return queue;
}
void delQueue(Queue *queue){
    free(queue->nums);
    free(queue);
}

int peek(){
    assert(size(queue) != 0);
    return queue->nums[queue->front];
}

void push(Queue *queue, int num){
    
}
```



