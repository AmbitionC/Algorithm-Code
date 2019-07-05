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

```javascript
function cutRope(n) {
    var hashMap = {
        "1": 0,
        "2": 1,
        "3": 2
    }
    // 特殊输入情况
    if (n < 1) {
        return 0
    }else if (n <= 3) {
        return hashMap[n]
    }
    var cutMap = {
        "0": 0,
        "1": 1,
        "2": 2,
        "3": 3
    }
    // 对输入的数值进行分解，对于n大于等于4的情况：
    for (let i = 4; i <= n; i ++) {
        let maxValue = 0;
        for( let j = 2; j < i; j ++) {
            let curValue = cutMap[j] * cutMap[i - j];
            if (curValue > maxValue) {
                maxValue = curValue;
            }
        }
        cutMap[i] = maxValue;
    }
    return cutMap[n];
}
```

2.采用贪婪算法，当n>=5时，尽可能多剪长度为3的绳子，剩下的绳子长度为4的时候，剪为两个长度为2的绳子

```javascript
function cutRope(n) {
    var hashMap = {
        "0": 0,
        "1": 0,
        "2": 1,
        "3": 2,
        "4": 4
    }
    // 特殊情况
    if (n < 1) {
        return 0
    }else if (n < 5) {
        return hashMap[n]
    }
    // 大于5的情况下尽可能多取3的长度的绳子，剩下为4时，剪为两个长度为2的绳子
    var count = 0
    while (n > 4) {
        n -= 3;
        count += 1;
    }
    var result = 0;
    result = Math.pow(3, count) * n
    return result;
}
```

#### 15. 二进制中1的个数

题目：输入一个整数，输出该数二进制表示中1的个数，其中负数用补码来表示

解决方案：为了避免输入的数字为负数，不采用右移的方式来判决
Way1: n与1按位与操作，判断最低位是否为1

```javascript
function NumberOf1(n) {
    // >>>表示不带符号向右移动二进制数，移动后前面统统补0；
    if (n < 0) {
        n = n >>> 0;
    }

    return testNum(n)

    function testNum(n) {
        var count = 0;
        var andSig = 1;
        strN = n.toString(2);
        for (var i = 0; i < strN.length; i ++) {
            let flag = n & andSig;
            if (flag != 0) {
                count ++;
            }
            andSig = andSig << 1;
        }
        return count
    }
}
```
Way2: 利用位运算的特性，用n与n-1按位与运算，如果有多少个1，则可以进行多少次这种运算

```javascript
function NumberOf1(n) {
    if (n < 0) {
        n = n >>> 0;
    }
    var count = 0;
    while (n) {
        n = n & (n - 1);
        count ++;
    }
    return count
}
```

#### 16. 数值的整数次方

题目： 给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

解决方案：该问题需要考虑以下问题：
（1）底数为0情况，返回值都为0
（2）指数为0的话，返回为1
（3）指数为负数的话，应该是倒数的情况

```javascript
function Power(base, exponent)
{
    // 特殊情况1，底数为0
    if (base == 0) {
        return 0
    }
    // 特殊情况2，指数为0
    if (exponent == 0) {
        return 1
    }
    // 特殊情况3，指数为负数
    if (exponent < 0) {
        let result = caculateExp(base, Math.abs(exponent));
        result = 1 / result;
        return result
    }
    return caculateExp(base, exponent)

    function caculateExp(base, exp) {
        let result = 1;
        for (let i = 0; i < exp; i ++) {
            result = result * base
        }
        return result
    }
}
```

```javascript
function Power(base, exponent){
    return Math.pow(base, exponent);
}
```

#### 17. 打印从1到最大的n位数

题目：输入数字n，按顺序打印出从1到最大的n位十进制数，比如输入3，则打印输出1，2，3一直到最大的3位数999

解决方案：在其他的编程语言中，需要注意大数的情况，但是对于js，其数据格式相对统一，只需要找到那个最大数，然后顺序打印即可

```javascript
function PrintNNumbers(n)
{
    var maxNumber = 0
    for (let i = 0; i < n; i ++) {
        maxNumber += 9 * Math.pow(10, i)
    }
    for (let i = 1; i <= maxNumber; i ++) {
        console.log(i)
    }
    return maxNumber
}
```

#### 18. 矩形覆盖问题

题目：我们可以用2\*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2\*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

解决思路：首先该问题为分类讨论问题
（1）矩形如果竖着放进去，那么剩下的放置方法为f(n-1)
（2）矩形如果横着放进去，那么剩下的放置方法为f(n-2)
=>也就是斐波那契数列问题

Way1. 采用hashMap方法
```javascript
function rectCover(number)
{
    // write code here
    var hashMap = {
        "1": 1,
        "2": 2,
    };
    if (number <= 0) {
        return 0
    }
    if (number >= 3) {
        for (let i = 3; i <= number; i ++) {
            hashMap[i] = hashMap[i - 1] + hashMap[i - 2];
        }
    }
    return hashMap[number]
}
```

Way2. 变量的解构赋值
```javascript
function rectCover(number)
{
    if (number == 0) {
        return 0
    }else if (number == 1) {
        return 1
    }else if (number == 2) {
        return 2
    }
    var [a, b, i] = [1, 2, 3];
    while (i <= number) {
        [b, a] = [a + b, b];
        i ++;
    }
    return b
}
```

#### 19*. 删除链表中的节点

题目：在O(1)时间内删除链表节点。给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间内删除该节点。链表节点与函数的定义已给出

解决方案：为了避免遍历所有节点，则先找到这个节点，然后将下一个节点的值复制到这个节点上，然后删除下一个节点，并改变该节点的指向。

```javascript
function ListNode(x){
    this.val = x;
    this.next = null;
}

function deleteNode(pHead, pNode)
{
    // 错误输入的情况下
    if (!pHead || !pNode) {
        return null;
    }
    // 删除的不是尾节点
    if (pNode.next !== null) {
        // 将下一个节点的值都赋到pNode上
        let pNext = pNode.next;
        pNode.val = pNext.val;
        pNode.next = pNext.next;
        // 删除pNext
        pNext = null
    }
    else if(pHead == pNode){
        // 链表只有一个节点，且就是要删除的节点
        pNode = null;
        pHead = null;
    }
    else {
        // 链表不止一个节点，要删除的节点就是最后一个节点，则需要遍历链表直到倒数第二个节点
        var tempNode = pHead;
        while (tempNode.next != pNode) {
            tempNode = tempNode.next
        }
        tempNode.next = null;
        pNode = null;
    }
    
}

```

#### 20*. 删除链表中的重复元素

题目：在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

解决方案：已知这个链表是排序的，那么就相邻的节点之间的比较

```javascript
function ListNode(x){
    this.val = x;
    this.next = null;
}

function deleteDuplication(pHead)
{
    // write code here
    // 遍历整个链表
    if (!pHead) {
        return null;
    }

    // 初始化三个指针
    var tempHead = new ListNode(-1);
    tempHead.next = pHead
    var preNode = tempHead;
    var curr1 = preNode.next;
    var curr2 = curr1.next;

    while(curr1) {
        if (!curr2 || curr2.val !== curr1.val) {
            // curr2与curr1不等的情况，或者curr2值为null的情况
            if (curr1.next !== curr2) {
                // 如果curr1的下一个节点不是curr2时
                clear(curr1, curr2);
                preNode.next = curr2;
            } else {
                preNode = curr1;
            }
            curr1 = curr2;
            if (curr2) {
                curr2 = curr2.next;
            }
        } else {
            if (curr2){
                curr2 = curr2.next;
            }
        }
    }
    return tempHead.next;

    function clear(node, stop) {
        var temp;
        while (node !== stop) {
            temp = node.next;
            node.next = null;
            node = temp;
        }
    }
}
```

#### 21. 正则表达式匹配

题目：请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab\*a"均不匹配

解决方案：
（其他语言）首先需要分类讨论：

1. 如果pattern的下一个字符不是“*”的时候，则如果str与patten字符匹配，则都向后移动一个字符。如不匹配则返回一个false

2. 如果pattern的下一个字符为“*”的时候，则有不同的匹配方式：

（1）pattern向后移动两个字符，因为“\*”和其前面的字符被忽略，因为“\*”可以匹配字符串中0个字符

（2）如果pattern和当前str的字符相匹配，那么str向后移动1个字符。那么也有两种相应的情况：

- 在pattern上向后移动两个字符

- 保持原有的模式不变


（JS）直接使用JS中的正则的语法即可实现

```javascript
function match(s, pattern) {
    if (s === "" && pattern === "") {
        // 输入都为空的情况
        return true
    }
    if (!pattern || pattern.length === 0) {
        return false
    }
    var reg = new RegExp("^" + pattern + "$")
    return reg.test(s)
}
```

#### 22. 表示数值的字符串

题目：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

解决方案：使用JS中的Number方法可以直接使用Number的方法

```javascript
function isNumeric(s) {
    return Number(s)
}
```

#### 23. 调整数组顺序使奇数位于偶数前面

题目：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

解决方案：（不要求相对位置不变的情况）定义两个指针，一个指向开头一个指向尾部，如果第一个指针为偶数且第二个指针为奇数时，交换两个数字

如果要求两者相对位置不变，则需要两个辅助数组，一个放奇数一个放偶数，然后进行合并

```javascript
function reOrderArray(array) {
    var oddArray = [];
    var evenArray = [];
    for (let i = 0; i < array.length; i ++) {
        if (array[i] % 2 == 1) {
            oddArray.push(array[i])
        }else {
            evenArray.push(array[i])
        }
    }
    return oddArray.concat(evenArray)
}
```

#### 24. 链表中倒数第k个节点

题目：输入一个链表，输出链表中倒数第k个节点

解决方案：为了防止两遍遍历链表，要找到倒数第k个点，也就是正数第list.length - k 的节点

```javascript
function FindKthToTail(head, k)
{
    // 计算整个链表的长度
    let tempNode = head;
    let count = 0;
    while (tempNode) {
        count ++;
        tempNode = tempNode.next
    }
    let number = count - k;
    // 排除特殊情况
    if (number < 0) {
        return null
    }
    // 找到倒数第k个点
    let cacheNode = head;
    while (number > 0) {
        cacheNode = cacheNode.next;
        number --;
    }
    return cacheNode
}
```

#### 25. 链表中环的入口节点

题目：给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

解决方案：解决链表中环的入口节点问题，主要分为以下三个步骤：

（1）判断链表有没有环（通过一前一后两个指针是否相遇来解决）

（2）计算环的长度（两个指针相遇在环中，则从该节点开始，通过计数得到环的长度）

（3）通过环的长度，通过两个指针来实现环的入口点确定。

```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function EntryNodeOfLoop(pHead)
{
    // write code here
    let loopResult = haveLoop(pHead);
    if (!loopResult){
        return null
    }else {
        let loopLength = loopNum(loopResult);
        let theEnterNode = enterNode(loopLength, pHead);
        return theEnterNode
    }
    
    function haveLoop(pHead){
        // Step1: 确定有没有环
        let curr1 = pHead;
        let curr2 = pHead;
        while (curr1.next) {
            curr1 = curr1.next;
            curr2 = curr2.next.next;
            if (curr1 === curr2){
                return curr1
            }
        }
        return false
    }
    
    function loopNum(meetNode) {
        // Step2: 计算环的长度
        let curr1 = meetNode;
        let curr2 = meetNode.next;
        let count = 1;
        while (curr1 !== curr2) {
            count ++;
            curr2 = curr2.next;
        }
        return count
    }
    
    function enterNode(loopLen, pHead) {
        let curr1 = pHead;
        let curr2 = pHead;
        for (let i = 0; i < loopLen; i ++) {
            curr2 = curr2.next
        }
        while (curr1.next) {
            if (curr1 === curr2) {
                return curr1
            }
            curr1 = curr1.next;
            curr2 = curr2.next;
        }
    }
}
```

#### 26. 反转链表

题目：输入一个链表，反转链表后，输出新链表的表头。

解决方案：反转链表需要保存三个状态，前一个节点，当前节点，以及下一个节点。这样是为了防止造成断裂。

```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function ReverseList(pHead)
{
    // write code here
    // 特殊情况，如果输入为空的情况
    if (!pHead) {
        return null
    }
    // 遍历链表
    let curNode = pHead;
    let preNode = null;
    while(curNode.next) {
        let nextNode = curNode.next;
        curNode.next = preNode;
        preNode = curNode;
        curNode = nextNode;
    }
    curNode.next = preNode;
    return curNode
}
```

#### 27. 合并两个排序的链表

题目： 输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

解决方案：从链表的头部开始进行比较，如果数值小，则先放入新的链表中

```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function Merge(pHead1, pHead2)
{
    // write code here
    // 排除特殊情况
    if (!pHead1) {
        return pHead2 ? pHead2 : null
    }else if (!pHead2) {
        return pHead1
    }
    
    // 比较每个链表的表头元素
    let curr1 = pHead1;
    let curr2 = pHead2;
    let result = new ListNode(-1);
    let curr = result;
    while (curr1 && curr2) {
        if (curr1.val < curr2.val) {
            curr.next = curr1;
            curr1 = curr1.next;
        } else {
            curr.next = curr2;
            curr2 = curr2.next;
        }
        curr = curr.next
    }
    if (curr1) {
        curr.next = curr1
    }
    if (curr2) {
        curr.next = curr2
    }
    
    curr = result.next;
    result.next = null;
    result = curr;
    // 防止内存泄漏
    curr = curr1 = curr2 = null;
    return result
}
```

#### 28. 树的子结构

题目：输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

解决方案：主要分为两个步骤

（1）首先在树A中找到与B根节点相同的那个节点（采用递归的方式）

（2）判断该节点下面的树的结构是否相同

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function HasSubtree(pRoot1, pRoot2)
{
    // write code here
    if (pRoot1 == null || pRoot2 == null) {
        return false
    }
    
    if (isTree1HasTree2(pRoot1, pRoot2)) {
        return true
    }else {
        return HasSubtree(pRoot1.left, pRoot2) || HasSubtree(pRoot1.right, pRoot2)
    }
    
    
    function isTree1HasTree2(pRoot1, pRoot2) {
        if (pRoot2 == null) {
            return true
        }
        if (pRoot1 == null) {
            return false
        }
        if (pRoot1.val !== pRoot2.val) {
            return false
        }
        if (pRoot1.val == pRoot2.val) {
            return isTree1HasTree2(pRoot1.left, pRoot2.left) && isTree1HasTree2(pRoot1.right, pRoot2.right)
        }
    }
}
```

#### 29. 二叉树的镜像

题目：操作给定的二叉树，将其变换为源二叉树的镜像。

解决方案：遍历树的所有节点，如果该节点有子节点，就交换其子节点，当交换完所有非叶节点的左右子节点之后就得到其镜像。

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function Mirror(root)
{
    // write code here
    if (root == null) {
        return null
    }
    let temp = root.left;
    root.left = root.right;
    root.right = temp;
    if (root.left){
        Mirror(root.left)
    };
    if (root.right) {
        Mirror(root.right)
    }
}
```

#### 30. 对称的二叉树

题目：请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

解决方案：在二叉树的镜像上基础上进行修改与判定即可

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function isSymmetrical(pRoot)
{
    // write code here
    if (!pRoot) {
        return true
    }
    
    return nodeSymmetrical(pRoot, pRoot)
    
    function nodeSymmetrical(node1, node2) {
        if (!node1 && !node2) {
            return true
        }
        if (!node1 || !node2) {
            return false
        }
        if (node1.val != node2.val) {
            return false
        }
        return nodeSymmetrical(node1.left, node2.right) && nodeSymmetrical(node1.right, node2.left)
    }
}
```

#### 31. 顺时针打印矩阵

题目：输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

解决方法：借助布尔值矩阵实现

```javascript
function printMatrix(matrix)
{
    // write code here
    let result = [];
    let rows = matrix.length;
    let cols = matrix[0].length;
    let hashMap = [];
    for (let i = 0; i < rows; i ++) {
        let hashPiece = [];
        for (let j = 0; j < cols; j ++) {
            hashPiece.push(false)
        }
        hashMap.push(hashPiece)
    }
    if (cols == 1 || rows == 1) {
        for (let i = 0; i < rows; i ++) {
            for (let j = 0; j < cols; j ++) {
                result.push(matrix[i][j])
            }
        }
    }
    var startX = 0;
    var startY = 0;
    while (result.length != cols * rows) {
        // 打印周期第一次
        for (let i = 0; i < cols; i ++) {
            if (!hashMap[startX][i]) {
                result.push(matrix[startX][i]);
                hashMap[startX][i] = true;
                startY = i;
            }
        }
        startX ++;
        // 打印周期第二次
        for (let i = 0; i < rows; i ++) {
            if (!hashMap[i][startY]) {
                result.push(matrix[i][startY]);
                hashMap[i][startY] = true;
                startX = i;
            }
        }
        startY --;
        // 打印周期第三次
        for (let i = 0; i < cols; i ++) {
            if (!hashMap[startX][cols-i-1]) {
                result.push(matrix[startX][cols-i-1]);
                hashMap[startX][cols-i-1] = true;
                startY = cols-i-1;
            }
        }
        startX --;
        // 打印周期第四次
        for (let i = 0; i < rows; i++) {
            if (!hashMap[rows-i-1][startY]) {
                result.push(matrix[rows-i-1][startY]);
                hashMap[rows-i-1][startY] = true;
                startX = rows-i-1;
            }
        }
        startY ++;
    }
    return result
}
```

#### 32. 包含min函数的栈

题目：定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

```javascript
var stack = [];
function push(node){
    stack.push(node);
}
function pop(){
    return stack.pop();
}
function top(){
    return stack[stack.length - 1];
}
function min(){
    return Math.min.apply(null, stack);
}
```

#### 33. 栈的压入、弹出序列

题目：输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

解决方法：规律如下
（1）如果下一个要弹出的数字刚好是栈顶数字，那么就直接弹出

（2）如果下一个要弹出的数字不在栈顶，则把压栈序列中还没有入栈的数字压入辅助栈，直到把下一个要弹出的数字压入栈顶为止

（3）如果所有数字都压入栈后，仍然没有找到下一个弹出数字，则返回false

```javascript
function IsPopOrder(pushV, popV)
{
    if (!pushV.length || !popV.length) {
        return false
    }
    let tempStack = [];
    let popIndex = 0;
    for (let i = 0; i < pushV.length;) {
        if (tempStack != [] && tempStack[tempStack.length - 1] == popV[popIndex]){
            popIndex ++;
            tempStack.pop();
        }else {
            tempStack.push(pushV[i]);
            i ++;
        }
    }
    while (popIndex <= popV.length && tempStack[tempStack.length - 1] == popV[popIndex]) {
        popIndex ++;
        tempStack.pop();
    }
    if (popIndex - 1 == popV.length) {
        return true
    }
    return false
}
```

#### 34. 从上到下打印二叉树

题目：从上往下打印出二叉树的每个节点，同层节点从左至右打印。

解决方法：引入一个队列，存放每一个节点的左右子节点，按照先进先出的原则来进行遍历

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function PrintFromTopToBottom(root)
{
    // write code here
    var result = [];
    var queue = [];
    if (root == null) {
        return result
    }
    queue.push(root);
    while (queue[0] != null) {
        let tempNode = queue.shift()
        result.push(tempNode.val);
        if (tempNode.left) {
            queue.push(tempNode.left)
        }
        if (tempNode.right) {
            queue.push(tempNode.right)
        }
    }
    return result
}
```

#### 35. 二叉搜索树的后序遍历序列

题目：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

解决方法：二叉搜索树的后序遍历的规律

（1）左子树节点的值，都比根节点的值小；

（2）右子树节点的值，都比根节点的值大。

```javascript
function VerifySquenceOfBST(sequence)
{
    // write code here
    let seqLength = sequence.length;
    if (seqLength == 0) {
        return false
    }
    let root = sequence[seqLength - 1];
    let leftTree = [];
    let rightTree = [];
    for (let i = 0; i < seqLength - 1; i ++) {
        if (sequence[i] > root) {
            leftTree = sequence.slice(0, i);
            rightTree = sequence.slice(i, seqLength - 1);
            break
        }
    }
    if (leftTree.length == 0 && rightTree.length == 0) {
        return true
    }
    if (leftTree.length > 0) {
        for (let i = 0; i < leftTree.length; i ++) {
            if (leftTree[i] >= root){
                return false
            }
        }
    }
    if (rightTree.length > 0) {
        for (let i = 0; i < rightTree.length; i ++) {
            if (rightTree[i] <= root){
                return false
            }
        }
    }
    return VerifySquenceOfBST(leftTree) || VerifySquenceOfBST(rightTree)
}
```

#### 36. 二叉树中和为某一值的路径

题目：输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)