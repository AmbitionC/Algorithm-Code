#### 1. 数组中重复的数字

题目：长度为n的数组所有的数字都在0到n-1中，数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。例如，输入长度为7的数组[2, 3, 1, 0, 2, 5, 3]，那么对应输出的第一个重复的数字2。                                               

解决方案1：采用哈希表来解决问题，扫描每一个数字，每一次扫描使用O(1)的时间判断哈希表是否已存在该数字。引入容错机制，如果输入为空或者数组长度为0，则返回错误。

```javascript
function duplicate(numbers, duplication)
{
    // write code here
    //这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    //函数返回True/False
    let numLength = numbers.length;
    let hashMap = {};
    for (let i = 0; i < numLength; i ++) {
        let arrNum = numbers[i]
        if (hashMap[arrNum] == 1) {
            duplication[0] = arrNum
            return true
        }else{
            hashMap[arrNum] = 1
        }
    }
    return false
}
```

解决方案2: 对数组进行重排，从头到尾扫描数组的每个数字，扫描下标为i的数字，首先比较这个数字（用m表示）是不是等于i，如果是，则继续扫描，如不是，则拿它和第m个数字进行比较，如果是，则找到第一个重复的数字；如果不相等，则把第i个数字和第m个数字交换，直到找到重复数字

```javascript
function duplicate(numbers, duplication)
{
    // write code here
    //这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    //函数返回True/False
    let numLength = numbers.length;
    for (let i = 0; i < numLength; ) {
        //如果相等
        let curNum = numbers[i]
        if (curNum == i){
            i ++;
        }else {
            if (curNum == numbers[curNum]) {
                duplication[0] = curNum;
                return true
            }else {
                numbers[i] = numbers[curNum];
                numbers[curNum] = curNum;
                i ++;
            }
        }
    }
    return false
}
```

#### 2. 二维数组的查找

题目：在一个二维数组中，每一行都按照从左到右递增的排序顺序，每一列都按照从上到下的顺序排序。请完成一个函数，输入一个二维数组和一个整数，判断该数组中是否含有该整数。

解决方案：从二维数组的右上角开始，如果右上角的数字大于该数字，则整列都不考虑，如果右上角的数字小于该数字，则整行都不考虑。因此可以采用循环的方式，循环的变化写在循环内部。

```javascript
function Find(target, array)
{
    // 从数组的右上角开始
    let rowLength = array.length;
    if (rowLength == 0) {
        return false
    }
    let colLength = array[0].length;
    let i = 0, j = colLength - 1;
    while(i < rowLength && j >= 0){
        if (target == array[i][j]) {
            return true
        }else if (target < array[i][j]) {
            j --;
        }else {
            i ++
        }
    }
    return false
}
```

#### 3. 替换空格

题目：实现一个函数，把字符串的每个空格都替换成“%20”。例如，当字符串为We Are Happy。则经过替换后的字符串为We%20Are%20Happy.

解决方案：使用JS的正则匹配的方式就可以实现

```javascript
function replaceSpace(str)
{
    return str.replace(/ /g, "%20")
}
```

#### 4. 从尾到头打印链表

题目：输入一个链表的头节点，从尾到头反过来打印每个节点的值

解决方案：使用栈的结构来实现，遍历链表，先进后出

```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function printListFromTailToHead(head)
{
    // 使用栈的结构来实现
    if (!head){
        return 0
    }else {
        let arrayList = [];
        while(head){
            arrayList.unshift(head.val);
            head = head.next;
        }
        return arrayList
    }
}
```

#### 5. 重建二叉树

题目：输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1, 2, 4, 7, 3, 5, 6, 8}和中序遍历序列{4, 7, 2, 1, 5, 3, 8, 6}，则重建二叉树并返回

解决方案：需要注意的是，前序遍历的第一个数为根结点，在中序遍历中找到这个树，就可以将树分为左右两个子树。采用递归的方式则可以确定整个树的结构。

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function reConstructBinaryTree(pre, vin)
{
    // write code here
    if (pre.lenght == 0 || vin.length == 0) {
        return null
    }
    let rootIndex = vin.indexOf(pre[0]);
    let leftVin = vin.slice(0, rootIndex);
    let leftPre = pre.slice(1, leftVin.length + 1);
    let rightVin = vin.slice(rootIndex + 1, vin.length + 1);
    let rightPre = pre.slice(leftVin.length + 1, pre.length + 1);
    let node = new TreeNode(pre[0]);
    node.left = reConstructBinaryTree(leftPre, leftVin);
    node.right = reConstructBinaryTree(rightPre, rightVin);
    return node;
}
```

#### 6. 二叉树的下一个节点

题目：给定一棵二叉树和其中的一个节点，如何找出中序遍历序列的下一个节点？树中的节点除了有两个分别指向左、右子节点的指针，还有一个指向父节点的指针。

解决方案：下面对这个中序遍历的下一个节点进行分类讨论

（1）如果该节点有右子树，那么它的下一个节点就是它的右子树的最左节点。也就是从右子节点出发，一直沿着指向左子节点指针。

（2）没有右子树的情况，又可以分为以下两种情况讨论：

- 没有右子树，且是其父节点的左子节点时，那么它的下一个节点就是其父节点

- 没有右子树，且是其父节点的右子节点时，那么就需要沿着父节点的指针向上找，直到找到一个节点a，这个节点a是其父节点的左子节点。那么下一个节点就是节点a的父节点

```javascript
/*function TreeLinkNode(x){
    this.val = x;
    this.left = null;
    this.right = null;
    this.next = null;
}*/
function GetNext(pNode)
{
    // 1.该节点有右子树，则找到右子树的最左子树
    // 2.该节点没有右子树，则分为以下两种情况：
    //   （1）该节点为父节点的左子节点，则父节点就为其下一节点
    //   （2）该节点为父节点的右子节点，则沿着父节点一直往上找直到该节点为父节点的左子节点
    if (!pNode) {
        // 输入为空的情况
        return pNode;
    }
    if (pNode.right) {
        // 找右子树的最左子树
        pNode = pNode.right;
        while (pNode.left) {
            pNode = pNode.left
        }
        return pNode
    }else if (pNode.next && pNode.next.left == pNode){
        // 该节点为父节点的左子节点，其父节点就是下一节点
        return pNode.next;
    }else if (pNode.next && pNode.next.right == pNode) {
        // 该节点为父节点的右子节点，则沿着父节点向上找，直到该节点为父节点的左子节点
        while (pNode.next && pNode.next.right == pNode) {
            pNode = pNode.next
        }
        return pNode.next
    }
}
```

#### 7. 用两个栈实现一个队列

题目：用两个栈实现一个队列，完成队列的Push和Pop操作，队列中元素为int类型

解决方案：用两个先进后出的栈实现一个队列，则stack1负责数据的压入，stack2负责数据的弹出。

需要注意的是，如果pop一个数字的时候，需要先将stack1中全部压入stack2，然后从stack2中弹出一个，**然后再压入stack1中**

```javascript
var stack1 = [];
var stack2 = [];
function push(node)
{
    // write code here
    stack1.push(node);
}
function pop()
{
    // write code here
    let temp = stack1.pop();
    while (temp){
        stack2.push(temp);
        temp = stack1.pop();
    }
    let result = stack2.pop();
    temp = stack2.pop();
    while(temp){
        stack1.push(temp);
        temp = stack2.pop();
    }
    return result
}
```

#### 8. 斐波那契数列

题目：输入一个整数n，求斐波那契数列的第n项

解决方案：对于斐波那契数列，如n=0，f(n)为0；如n=1，f(n)为1；如n>1，f(n)=f(n-1)+f(n-2)
Way1（不推荐）: 采用递归的方式来计算得到

```javascript
function Fibonacci(n)
{
    // write code here
    if (n == 0){
        return 0
    }else if (n == 1){
        return 1
    }else{
        let result = Fibonacci(n-1) + Fibonacci(n-2);
        return result
    }
}
```

Way2（推荐）: 从下往上计算，保存每一次计算的结果，在计算下一次的时候进行调用

采用hashMap来存储数据
```javascript
function Fibonacci(n)
{
    // write code here
    if (n == 0){
        return 0
    }else if (n == 1){
        return 1
    }else{
        let hashMap = {
            "0": 0,
            "1": 1
        };
        for (let i = 2; i < n + 1; i++) {
            //构建hashMap
            hashMap[i] = hashMap[i-1] + hashMap[i-2];
        }
        return hashMap[n]
    }
}
```

但是HashMap也需要占用O(n)的内存，因此建议使用两个变量来进行存储即可
```javascript
function Fibonacci(n)
{
    // write code here
    if (n == 0){
        return 0
    }else if (n == 1){
        return 1
    }else{
        let a = 1, b = 0, i = 2, result;
        while(i <= n){
            result = a + b;
            b = a;
            a = result;
            i ++;
        }
        return result
    }
}
```

#### 9. 青蛙跳台阶问题

题目：一只青蛙可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上n级台阶总共有多少种跳法？

解决方案：首先需要归纳该问题，从头开始举例子，然后发现规律。
得到的规律为：青蛙第一次跳有两种情况，一种是跳一级，则剩下n-1级有f(n-1)种跳法；一种是跳两级，则剩下n-2级有f(n-2)种跳法。归纳为f(n) = f(n-1) + f(n-2)

```javascript
function jumpFloor(number)
{
    // write code here
    if (number == 0){
        return 0
    }else if (number == 1){
        return 1
    }else{
        let hashMap = {
            "0": 1,
            "1": 1
        };
        for (let i = 2; i < number + 1; i++) {
            //构建hashMap
            hashMap[i] = hashMap[i-1] + hashMap[i-2];
        }
        return hashMap[number]
    }
}
```

#### 10. 青蛙跳台阶问题2

题目：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶......它也可以跳上n级。总共有n级台阶，一共有多少种跳法？

解决方案：对于n个台阶，每个台阶有两种状态：跳或者不跳，所以一共2^n种跳法

```javascript
function jumpFloorII(number)
{
    // write code here
    // 对于每个台阶有两种情况，跳和不跳，因此有
    if (number == 0){
        return 0
    }else if(number == 1){
        return 1
    }else{
        return Math.pow(2, (number-1));
    }
}
```

#### 11. 旋转数组

题目：将一个数组最开始的几个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素，例如数组[3,4,5,1,2]为[1,2,3,4,5]的一个旋转，该数组的最小值为1。Note: 给出的所有元素都大于0，若数组的大小分为0，请返回0

解决方案：1. 该排序的数组可以使用二分法进行选择，初始化两个指针，第一个与最后一个，然后找到中间的元素，如果中间的元素大于第一个指针，则更新第一个指针，如中间元素小于第一个指针，则更新第二个指针。
该方法需要注意两个特殊情况：
- 数组本身是排序的，则直接返回第一个即可
- 还有一个特殊情况：[0, 1, 1, 1, 1]

```javascript
function minNumberInRotateArray(rotateArray)
{
    // write code here
    // 针对特殊情况
    if (rotateArray.length == 0){
        // 输入数组长度为0的情况
        return 0
    }
    while(rotateArray.length > 2){
        // 取中间的数
        var middleNum = parseInt(rotateArray.length / 2);
        var tempArray = [];
        tempArray.push(...rotateArray);
        if (tempArray[middleNum] >= rotateArray[0]){
            // 中间值大于第一个指针
            rotateArray.splice(0, middleNum);
        }
        if (tempArray[middleNum] <= rotateArray[tempArray.length - 1]){
            // 中间值小于等于第二个指针
            let cutNum = tempArray.length - middleNum;
            rotateArray.splice(middleNum + 1, cutNum);
        }
    }
    return rotateArray[1];
}
```

2. 直接使用JS中的Math.min方式

```javascript
function minNumberInRotateArray(rotateArray)
{
    // write code here
    // 针对特殊情况
    if (rotateArray.length == 0){
        // 输入数组长度为0的情况
        return 0
    }
    let minNum = Math.min(...rotateArray)
    return minNum
}
```

#### 12. 矩阵中的路径

题目：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

解决方案：首先构建布尔值矩阵来判断其路径是否为走过。其次采用回溯法来判断路径

```javascript
function hasPath(matrix, rows, cols, path)
{
    // 初始化二维数组
    var visited = [];
    for (let i = 0; i < rows; i ++) {
        for (let j = 0; j < cols; j ++) {
            visited[i*cols+j] = false;
        }
    }

    var pathLen = 0;
    var resultAll = 0

    // 对Matrix中的每一个进行验证
    for (let i = 0; i < rows; i ++) {
        for (let j = 0; j < cols; j ++) {
            var result = judgeEvery(i, j);
            resultAll += result
        }
    }
    return Boolean(resultAll);
    
    // 判断每一个输入
    function judgeEvery(row, col){
        if (pathLen === path.length) {
            return true
        }
        var hasPath = false;
        if (row >= 0 && row <= rows && col >= 0 && col <= cols && matrix[row*cols+col] === path[pathLen] && !visited[row*cols+col]) {
            pathLen ++;
            visited[row*cols+col] = true;
            hasPath = judgeEvery(row+1, col) + judgeEvery(row-1, col) + judgeEvery(row, col-1) + judgeEvery(row, col+1);
            if (!hasPath) {
                pathLen --;
                visited[row*cols+col] = false;
            }
        }
        return hasPath
    }
}
```

#### 13. 机器人的运动范围

题目：地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

解决方案：采用回溯法则可以得到机器人的运动范围

```javascript
function movingCount(threshold, rows, cols)
{
    // write code here
    // 错误输入的情况
    if (threshold < 0 || rows < 0 || cols < 0) {
        return 0
    }

    // 构建布尔值矩阵
    var boolMatrix = [];
    for (let i = 0; i < rows*cols; i ++) {
        boolMatrix[i] = false
    }

    // 产生数位之和的矩阵
    var matrix = [];
    for (let i = 0; i < rows; i ++) {
        for (let j = 0; j < cols; j ++) {
            matrix[i*cols+j] = caculateSum(i, j);
        }
    }

    var count = findCoverage(0, 0);

    return count;
    
    function findCoverage(row, col) {
        var pathCount = 0;
        if (row >= 0 && row <= rows && col >= 0 && col <= cols && matrix[row*cols+col] <= threshold && !(boolMatrix[row*cols+col])) {
            boolMatrix[row*cols+col] = true;
            pathCount = 1 + findCoverage(row + 1, col) + findCoverage(row - 1, col) + findCoverage(row, col + 1) + findCoverage(row, col - 1)
        }
        return pathCount
    }

    // 计算当前行和列的数位之和
    function caculateSum(row, col) {
        let rowCounts = digits(row);
        let colCounts = digits(col);
        let rowSum = 0;
        let colSum = 0;
        for (let i = 0; i < rowCounts; i ++) {
            rowSum += parseInt(row % 10);
            row = parseInt(row / 10);
        }
        for (let j = 0; j < colCounts; j ++) {
            colSum += parseInt(col % 10);
            col = parseInt(col / 10);
        }
        return (rowSum + colSum)
    }

    // 计算输入数字的位数
    function digits(number) {
        var count = 1;
        while (number >= 10) {
            number = parseInt(number / 10);
            count ++;
        }
        return count
    }
}
```

#### 14. 剪绳子问题

题目：给定一根长为n的绳子，请把绳子剪成m段（m, n为整，且n>1, m>1），每段绳子的长度为k[0], k[1],...,k[m]，请问k[0]*k[1],...,k[m]的最大乘积是多少？当绳子的长度是8时，得到的最大乘积为18

解决方案：1.采用动态规划的方式，先计算f(2)，f(3)的情况，然后计算n>4的情况。f(n)=f(i)*f(n-i)
2.采用贪婪算法，当n>=5时，尽可能多剪长度为3的绳子，剩下的绳子长度为4的时候，剪为两个长度为2的绳子

#### 15. 二进制中1的个数

题目：输入一个整数，输出该数二进制表示中1的个数，其中负数用补码来表示

解决方案：