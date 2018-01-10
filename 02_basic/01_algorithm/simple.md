[TOC]

# 广度和深度

---

> **广度优先搜索规则:**
>
> 1. 访问下一个邻接的未访问过的节点，这个顶点必须是当前节点的邻接点，标记它，并把它插入到队列中。
> 2. 如果无法执行规则 1，那么就从队列头取出一个顶点，并使其作为当前顶点。
> 3. 当队列为空不能执行规则2时，就完成了整个搜索过程。
>
> **深度优先搜索规则:**
>
> 1. 如果可能，访问一个邻接的未访问过的顶点，标记它，并把它放入栈中。
> 2. 当不能执行规则1时，如果栈不能为空，就从栈中弹出一个顶点。
> 3. 当不能执行规则1和规则2时，就完成了整个搜索过程



```c
void DFS(Node root) { // 非递归实现
    stack<Node> s;
    root.visited = true;
    printf("%d", root.val); // 访问
    s.push(root);           // 入栈
    while (!s.empty()) {
        Node pre = s.top(); // 取栈顶顶点
        int j;
        for (j = 0; j < pre.adjacent.size(); j++) { // 英语课给你顶点j相邻的顶点
            Node cur = pre.adjacent[j];
            if (cur.visited == false) {
                printf("%d", cur.val); // 访问
                cur.visited = true;
                s.push(cur);        // 访问完后入栈
                break;              // 找到一个相邻未访问的顶点，访问之后则跳出循环
            }
        }
        // 对于节点4，找完所有节点发现都已访问过或者没有邻边，所以j此时=节点总数，然后把这个4给弹出来
        // 直到弹出1，之前的深度搜索的值都已弹出，有半部分还没有遍历，开始遍历有阗
        if (j == pre.adjacent.size()) { // 如果与i相邻的顶点都被访问过，则将顶点i出栈
            s.pop();
        }
    }
}

void BSF(Node root) {
    queue<Node> q;
    root.visited = true;
    printf("%d ", root.val); // 访问
    q.push(root);
    while (!q.empty()) {
        Node pre = q.front();
        q.pop();
        for (Node cur : pre.adjacent) {
            if (cur.visited == false) {
                printf("%d ", cur.val);
                cur.visited = true;
                q.push(cur);
            }
        }
    }
}
```

# 最小生成树

> 1. 连接每个顶点最少的连线。最小生成树边的数量总是比顶点的数量少1

# # topology sort

[parallel job scheduling](http://www.codebytes.in/2015/11/parallel-job-scheduling-using.html)

[avl tree](https://courses.cs.washington.edu/courses/cse332/16sp/exams/cse332-final-12wi.pdf)

[Implementing parallel topological sort in a java graph library](http://fmt.cs.utwente.nl/files/sprojects/64.pdf)

























)