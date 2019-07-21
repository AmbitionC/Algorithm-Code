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

#### 36*. 二叉树中和为某一值的路径

题目：输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

解决方法：采用深度优先搜索(dfs)方法来遍历树的结构

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function FindPath(root, expectNumber)
{
    // write code here
    //采用深度遍历的方式实现
    var result = [];
    var temp = [];
    dfs(root, 0);
    return result
    
    function dfs(root, sum) {
        if (!root) {
            return;
        }
        temp.push(root.val);
        sum += root.val;
        if (!root.left && !root.right && sum === expectNumber) {
            result.push(temp.concat());
        }
        if (root.left) {
            dfs(root.left, sum);
        }
        if (root.right) {
            dfs(root.right, sum)
        }
        temp.pop();
    }
}
```

#### 37*. 复杂链表的复制

题目：输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

解决方法：

1. 采用传统的方式，分为以下步骤

（1）复制原有链表上的节点N，创建复制的节点N'，并将复制出来的节点N'用pNext连接起来

（2）创建配对信息< N, N' >, 并放入哈希表

（3）复制每一个节点的pRandom

2. 采用递归的方式实现链表的复制

```javascript
/*function RandomListNode(x){
    this.label = x;
    this.next = null;
    this.random = null;
}*/
function Clone(pHead)
{
    // write code here
    if (!pHead) {
        return null
    }
    var resultHead = new RandomListNode(pHead.label);
    resultHead.random = pHead.random;
    resultHead.next = Clone(pHead.next);
    return resultHead
}
```

#### 38. 二叉搜索树与双向链表

题目：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

解决方案：首先需要找到整个树的最后一个节点，同时对于子树也需要找到最后一个节点，并连接起来，形成一个双向链表。最后再指向双向链表的第一个节点即可

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function Convert(pRootOfTree)
{
    // write code here
    // 找到最后一个节点
    var lastNode = ConvertNode(pRootOfTree);
    var pHead = lastNode;
    while(pHead && pHead.left) {
        pHead = pHead.left
    }
    return pHead
    
    function ConvertNode(node) {
        if (!node) {
            return ;
        }
        if (node.left) {
            lastNode = ConvertNode(node.left);
        }
        node.left = lastNode;
        if (lastNode) {
            lastNode.right = node;
        }
        lastNode = node;
        if (node.right) {
            lastNode = ConvertNode(node.right);
        }
        return lastNode
    }
}
```

#### 39*. 序列化二叉树

题目：请实现两个函数，分别用来序列化和反序列化二叉树

解决方案：序列化与反序列化的步骤如下

1. 采用前序遍历的方式实现序列化二叉树

2. 采用递归的方式从左到右，将序列转化为二叉树

```javascript
/*function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
}*/

function Serialize(pRoot)
{
    // 采用前序遍历的方式实现序列化二叉树
    var sequence = [];
    ser(pRoot);
    // 将数组形式的sequence转变为序列
    
    return sequence;
    
    function ser(node) {
        if (!node){
            sequence.push('#')
            return ;
        }
        sequence.push(node.val);
        ser(node.left);
        ser(node.right);
    }
}

function Deserialize(str)
{
    // 将前序遍历的数组调整为序列化二叉树
    // 首先将输入进来的字符串转变为数组
    let pRoot = null;
    let temp = str.shift();
    if (temp !== '#') {
        pRoot = new TreeNode(temp);
    } else {
        return pRoot;
    }
    pRoot.left = Deserialize(str);
    pRoot.right = Deserialize(str);
    return pRoot
    
}
```

#### 40. 字符串的排列

题目：输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

解决方案：首先确定第一个字符，然后列举剩下的所有的字符的排列顺序

```javascript
function Permutation(str)
{
    // write code here
    var result = [];
    if (str.length === 0) {
        return result;
    }
    str = str.split("").sort();
    var resultPiece = "";
    permutate(str, resultPiece, result);
    return result;

    function permutate(strLeft, resultPiece, result) {
        if (strLeft.length === 0) {
            result.push(resultPiece)
        }
        for (let i = 0; i < strLeft.length; i ++) {
            // 排除重复数字的情况
            if (strLeft[i] === strLeft[i + 1]) {
                continue;
            }
            let c = strLeft.splice(i , 1)[0];
            permutate(strLeft, resultPiece+c, result);
            // 下一次循环前还原数组
            strLeft.splice(i, 0, c);
        }
    }
}
```

#### 41. 数组中出现次数超过一半的数字

题目：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

解决方案：本题采用hashMap来实现，只需要遍历一次数组，即可实现。时间复杂度为O(n)

```javascript
function MoreThanHalfNum_Solution(numbers)
{
    // write code here
    // 构建Hash表
    var hashMap = {};
    var length = numbers.length;
    var halfLength = Math.floor(length / 2);
    if (length === 0) {
        return 0
    }
    for (let i = 0; i <= length; i ++) {
        if (!hashMap[numbers[i]]) {
            hashMap[numbers[i]] = 1;
        }else {
            hashMap[numbers[i]] += 1;
        }
        if (hashMap[numbers[i]] > halfLength) {
            return numbers[i]
        }
    }
    return 0
}
```

#### 42. 最小k个数

题目：输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

解决方案：使用JS中的min函数实现最小的数并放入result，改变数组，进行循环即可

```javascript
function GetLeastNumbers_Solution(input, k)
{
    // write code here
    var result = [];
    if (input.length === 0) {
        return result
    }
    if (input.length < k) {
        return result
    }
    for (let i = 0; i < k; i ++) {
        let temp = Math.min.apply(null, input);
        result.push(temp);
        input.splice(input.indexOf(temp), 1);
    }
    return result
}
```

#### 43. 数据流中的中位数

题目：如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

解决方案：直接实现即可，对于数组的排序sort()，需要注意其用法

```javascript
var sortedArray = []
function Insert(num)
{
    // write code here
    sortedArray.push(num);
    sortedArray.sort((a, b) => a - b);
    return sortedArray
}
function GetMedian(){
	// write code here
    // 判断数组的长度为奇数还是偶数
    midNum = Math.floor(sortedArray.length / 2);
    if (sortedArray.length & 1) {
        // 为奇数的情况
        return sortedArray[midNum]
    }else {
        // 偶数的情况
        return (sortedArray[midNum] + sortedArray[midNum - 1]) / 2
    }
}
```

#### 44. 连续子数组的最大和

题目：
HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)

解决方案：从第一位开始，进行叠加，如果出现小于0的情况，则从下一位开始。属于采用动态规划的方法。

```javascript
function FindGreatestSumOfSubArray(array)
{
    // write code here
    if (array.length === 0) {
        return null
    }
    var sum = array[0];
    var tempSum = array[0];
    for (let i = 1; i < array.length; i ++) {
        tempSum = tempSum > 0 ? array[i] + tempSum : array[i];
        sum = tempSum > sum ? tempSum : sum;
    }
    return sum
}
```

#### 45. 1～n整数中1出现的次数

题目：求出1\~13的整数中1出现的次数,并算出100\~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

```javascript
function NumberOf1Between1AndN_Solution(n)
{
    // write code here
    var count = 0;
    for (let i = 1; i <= n; i ++) {
        numberOf1(i)
    }
    return count
    // 计算单个数字的1出现次数
    function numberOf1(num) {
        while (num) {
            if (num % 10 === 1) {
                count ++;
            }
            num = Math.floor(num / 10);
        }
    }
}
```

```javascript
// 采用正则的方式匹配
function NumberOf1Between1AndN_Solution(n)
{
    if (n < 0) return 0;
    var ones = 0;
    var arr = [];
    while(n){
        arr.push(n);
        n--;
    }
    return arr.join('').replace(/[^1]+/g,'').length;
}
```

#### 46. 把数组排成最小的数

题目：输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

解决思路：比较两个数的大小，然后进行排序

```javascript
function PrintMinNumber(numbers)
{
    // write code here
    // 对数组进行排序
    var sortedArr = [];
    sortedArr.push(numbers[0]);
    for (let i = 1; i < numbers.length; i ++) {
        for (let j = 0; j < sortedArr.length; j ++) {
            let comResult = compareNums(numbers[i], sortedArr[j]);
            if (!comResult) {
                sortedArr.splice(j, 0, numbers[i])
                break;
            }
        }
        let lastResult = compareNums(numbers[i], sortedArr[sortedArr.length - 1]);
        if (lastResult) {
            sortedArr.push(numbers[i])
        }
    }

    // 将排序后的数组形成最小数
    let result = sortedArr.join('')
    return result

    // 比较两个数的大小
    function compareNums(a, b) {
        let strA = a.toString() + b.toString();
        let strB = b.toString() + a.toString();
        return parseInt(strA) >= parseInt(strB) ? true : false
    }
}
```

#### 47. 把数字翻译成字符串

题目：给定一个数字，我们按照如下规则将其翻译为字符串：0翻译成“a”, 1翻译成“b”,..., 11翻译成“l”,..., 25翻译成“z”。一个数字可能有多个翻译。例如，12258有5种不同的翻译，分别是“bccfi”、“bwfi”、“bczi”、“mcfi”和“mzi”。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

解决思路：从右边到左边，采用递归的方式得到结果

```javascript
var transferMap = {
    "0": "a", "1": "b", "2": "c", "3": "d", "4": "e", "5": "f", "6": "g", "7": "h", "8": "i", "9": "j", "10": "k", "11": "l", "12": "m", "13": "n", "14": "o", "15": "p", "16": "q", "17": "r", "18": "s", "19": "t", "20": "u", "21": "v", "22": "w", "23": "x", "24": "y", "25": "z"
}
var count = 0

function transferNumToStr(number) {

    if (number === 0) {
        count ++;
        return;
    }

    combineStr(number);

    function combineStr(num) {
        // 末位数为单个位数
        lastNum = num % 10;
        restNums = Math.floor(num / 10);
        transferNumToStr(restNums);

        // 末位数为两位数的情况
        lastNum = num % 100;
        if (lastNum >= 26 || (num / 10) < 1) {
            return;
        }else {
            restNums = Math.floor(num / 100);
            transferNumToStr(restNums)
        }
    }
}

transferNumToStr(1225);
console.log(count)
```

#### 48. 礼物的最大价值

题目：在一个m*n的棋盘，每一格都放有一个礼物，每个礼物都有一定的价值（价值大于0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格。直到拿到棋盘的右下角。给定一个棋盘及上面的礼物，请计算你最多能拿到多少价值的礼物？

解决方案：采用动态规划的方法实现，递归的公式表达如下：

```
f(i, j) = max(f(i-1, j), f(i, j-1)) + gift[i, j]
```

#### 49*. 最长不含重复字符的子字符串

题目：请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。假设字符串中只包含'a'~'z'的字符。例如在字符串"arabcacfr"中，最长的不含重复字符的子字符串是"acfr"，长度为4。

解决方案：Way1. 采用从头到尾遍历的方式实现，时间复杂度为O(n^2)

```js
function noDuplicateStr(str) {
    let hashMap = {};
    let maxResult = [];
    for (let i = 0; i < str.length; i ++) {
        resCache = [];
        for (let j = i; j < str.length; j ++) {
            if (hashMap[str[j]] === 1) {
                if (resCache.length >= maxResult.length) {
                    maxResult = resCache;
                }
            }else {
                hashMap[str[j]] = 1;
                resCache.push(str[j])
            }
        }

        hashMap[str[i]] = 1;
    }
    return maxResult
}

var testStr = "arabcacfr"
noDuplicateStr(testStr);
```

Way2. 采用动态规划的方式实现

#### 50. 丑数

题目：把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

解决方案：Way1. 通过逐一判断的方式实现

```js
function GetUglyNumber_Solution(index)
{
    // write code here
    if (index <= 0) {
        return 0;
    }
    let [count, number] = [0, 0]
    while (count <= index) {
        number ++;
        if (isUgly(number)) {
            count ++;
        }
    }
    return number

    function isUgly(number) {
        while (number % 2 == 0) {
            number /= 2;
        }
        while (number % 3 == 0) {
            number /= 3;
        }
        while (number % 5 == 0) {
            number /= 5;
        }
        return (number == 1) ? true : false
    }
}
```
该方案导致解决方法的时间复杂度过大

Way2. 创建数组保存已经找到的数组
丑数由另一个丑数乘以2，3，5得到的结果

```js
function GetUglyNumber_Solution(index)
{
    // write code here
    if (index <= 0) {
        return 0
    }
    var uglyNum = [1];
    var [M2, M3, M5] = [0, 0, 0];
    for (let i = 1; i < index; i ++) {
        uglyNum[i] = Math.min(uglyNum[M2] * 2, uglyNum[M3] * 3, uglyNum[M5] * 5);
        if (uglyNum[i] === uglyNum[M2] * 2) M2 ++;
        if (uglyNum[i] === uglyNum[M3] * 3) M3 ++;
        if (uglyNum[i] === uglyNum[M5] * 5) M5 ++;
    }
    // console.log(uglyNum)
    return uglyNum[index - 1]
}
```

#### 51. 第一个只出现一次的字符

题目：在一个字符串（0 <= 字符串长度 <= 10000, 全部由字母组成）中找到第一个只出现一次的字符，并返回它的位置，如没有则返回-1（需要区分大小写）

解决方案：构建hash表，首先遍历一次字符串，将重复出现的放入。然后再在hashMap中查找

```js
function FirstNotRepeatingChar(str)
{
    // 构建hash表
    if (str.length == 0) {
        return -1
    }
    var hashMap = {};
    for (let i = 0; i < str.length; i ++) {
        if (hashMap[str[i]] == null) {
            hashMap[str[i]] = i;
        }else {
            hashMap[str[i]] -= i;
        }
    }   
    for (let j = 0; j < str.length; j ++) {
        if (hashMap[str[j]] >= 0) {
            return hashMap[str[j]]
        }
    } 
}
```

#### 52*. 数组中的逆序对

题目：在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

解决方案：为了减小时间复杂度，先将数组分割为子数组，统计出子数组内部的逆序对的数目，然后统计两个相邻的子数组之间的逆序对的数目。该方法排序的原理就是归并排序，时间复杂度为O(nlogn)

```js
function InversePairs(data)
{
    let len = data.length;
    if (len === 0) {
        return 0;
    }
    let copy = data.slice();
    let res = InversePairsCore(data, copy, 0, len-1);
    delete copy;
    return res%1000000007;
}
function InversePairsCore(data, copy, start, end) {
    if (start === end) {
        return 0;
    }

    let length = (end - start) >> 1;
    let left = arguments.callee(copy, data, start, start+length);
    let right = arguments.callee(copy, data, start+length+1, end);

    let i = start + length;
    let j = end;
    let indexCopy = end;
    let cnt = 0;
  
    while (i >= start && j >= start+length+1) {
        if (data[i] > data[j]) {
            copy[indexCopy--] = data[i--];
            cnt += j - start - length;
        } else {
            copy[indexCopy--] = data[j--];
        }
    }
    for (; i >= start; i --) {
        copy[indexCopy--] = data[i];
    }
    for (; j >= start + length + 1; j --) {
        copy[indexCopy--] = data[j];
    }
    return left + right + cnt;
}
```

#### 53. 两个链表的第一个公共节点

题目：输入两个链表，找出它们的第一个公共结点。

解决方案：
Way1. 采用两层遍历循环的方式来找到公共的结点

```js
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function FindFirstCommonNode(pHead1, pHead2)
{
    // write code here
    if(!pHead1 || !pHead2) {
        return null
    }
    var start2 = pHead2;
    while (pHead1) {
        pHead2 = start2;
        while (pHead2) {
            if (pHead1 == pHead2) {
                return pHead1;
            } else {
                pHead2 = pHead2.next;
            }
        }
        pHead1 = pHead1.next;
    }
    return null
}
```

Way2. 计算两个链表的长度，计算两个链表的差值diffLen，然后让长的链表先进行移动diffLen，然后同步进行移动查找即可

```js
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function FindFirstCommonNode(pHead1, pHead2)
{
    let listLen1 = caculateLength(pHead1);
    let listLen2 = caculateLength(pHead2);
    let diffLen = listLen1 - listLen2;
    // 长链表
    let curr1 = pHead1;
    let curr2 = pHead2;
    if (listLen2 > listLen1) {
        diffLen = listLen2 - listLen1;
        curr1 = pHead2;
        curr2 = pHead1;
    }
    // 让长的链表先进行移动
    for (let i = 0; i < diffLen; i ++) {
        curr1 = curr1.next;
    }
    // 让两个链表共同移动
    while (curr1 && curr2 && curr1 != curr2) {
        curr1 = curr1.next;
        curr2 = curr2.next;
    }
    return curr1
    
    // 计算链表的长度
    function caculateLength(pHead) {
        let count = 0;
        while (pHead) {
            count ++;
            pHead = pHead.next;
        }
        return count;
    }
}
```

#### 54. 数字在排序数组中出现的次数

题目：统计一个数字在排序数组中出现的次数。

解决方案：Way1. 对于数组是排序的话，首先可以通过二分法对该数字进行定位，然后找到该数字后，再从左和从右统计出现的次数

```js
function GetNumberOfK(data, k)
{
    // write code here
   var result = 0;
    findMiddle(data, k);

    // 找该数字两边共多少个
    if (result != null) {
        let left = right = result;
        count = 1;
        for (let i = left - 1; i >= 0; i --) {
            if (data[i] != data[result]) {
                break
            }
            count ++;
        }
        for (let j = right + 1; j < data.length; j ++) {
            if (data[j] != data[result]) {
                break
            }
            count ++;
        }
        return count;
    } else {
        return 0;
    }
    

    // 采用二分法对数组中的数字进行定位，找到该数字的index
    function findMiddle(arr, k) {
        if (arr.length == 0) {
            result = null
        }
        let midIndex = Math.floor(arr.length / 2);        
        if (arr[midIndex] > k) {
            findMiddle(arr.slice(0, midIndex), k);
        }else if (arr[midIndex] < k) {
            result += midIndex + 1;
            findMiddle(arr.slice(midIndex + 1), k);
        }else if (arr[midIndex] == k){
            result += midIndex;
            return ;
        }
    }
}
```

Way2. 使用JS的reduce方法，代码简洁，但是时间、空间复杂度比Way1高
```js
function GetNumberOfK(data, k)
{
    // write code here
   return data.reduce(function(count, a) {
        return a === k ? count + 1 : count
    }, 0) 
}
```

#### 55. 二叉搜索树的第k个结点

题目：给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

解决方案：因为是二叉搜索树，其性质是左子结点<右子结点，无论是选择第k大的结点或者是第k小的结点，只需要做出二叉搜索树的中序遍历的结果即可。

```js
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function KthNode(pRoot, k)
{
    if (!pRoot || k <= 0) {
        return null
    }
    var listTree = [];
    var result;
    middleTree(pRoot);
    return result

    function middleTree(root) {
        if (root.left !== null) {
            middleTree(root.left);
            root.left = null;
        }
        if (root.left === null && root.right === null) {
            listTree.push(root.val);
            if (listTree.length == (k)) {
                result = root;
                return ;
            }
        }
        if (root.left === null && root.right !== null) {
            listTree.push(root.val);
            if (listTree.length == (k)) {
                result = root;
                return ;
            }
            middleTree(root.right);
            root.right = null;
        }
    }
}
```

```js
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function KthNode(pRoot, k)
{
    if(!pRoot || !k){
        return null;
    }
    return KthCore(pRoot);
    function KthCore(node){
        var target = null;
        if(node.left){
            target = KthCore(node.left);
        }
        if(!target){
            if(k === 1)
                target = node.val;
            k--;
        }
        if(!target && node.right)
            target = KthCore(node.right);
        return target;
    }
}
```

#### 56. 二叉树的深度

题目：输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

解决方案：通过递归的方法来实现，如果左子树的深度大于右子树的深度，则左子树的深度加1即为整个树的最大深度。通过这种递归的计算方式来实现。

```js
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function TreeDepth(pRoot)
{
    // write code here
    if (!pRoot) {
        return 0;
    }
    // 通过剪枝的方式来实现
    var maxDepth = 0;
    var count = 1;
    caculateDep(pRoot);
    return maxDepth;
    function caculateDep(root) {
        if (root.left) {
            count ++;
            caculateDep(root.left);
            root.left = null;
        }
        if (!root.left && root.right) {
            count ++;
            caculateDep(root.right);
            root.right = null;
        }
        if (!root.left && !root.right) {
            maxDepth = (count > maxDepth ? count : maxDepth);
            // 剪枝
            count --;
            root = null;
        }
    }
}
```

采用dfs
```js
function TreeDepth(pRoot){
    if(!pRoot){
        return 0;
    }
    var depth = 0;
    var currDepth = 0;
    dfs(pRoot);
    return depth;
    function dfs(node){
        if(!node){
            depth = depth > currDepth ? depth : currDepth;
            return;
        }
        currDepth++;
        dfs(node.left);
        dfs(node.right);
        currDepth--;
    }
}
```

#### 57. 平衡二叉树

题目：输入一棵二叉树，判断该二叉树是否是平衡二叉树。某二叉树的任一结点的左右子树深度相差不超过1，则其为平衡二叉树。

解决方案：

Way1. 采用多次遍历的方式实现，但是该方法的时间复杂度较高
```js
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function IsBalanced_Solution(pRoot)
{
    // write code here
    if (!pRoot) {
        return true
    }
    var left = TreeDepth(pRoot.left);
    var right = TreeDepth(pRoot.right);
    if ((left - right) > 1 || (left - right) < -1) {
        return false
    }
    return IsBalanced_Solution(pRoot.left) && IsBalanced_Solution(pRoot.right)

    function TreeDepth(pRoot) {
        // write code here
        if (!pRoot) {
            return 0;
        }
        // 通过剪枝的方式来实现
        var maxDepth = 0;
        var count = 1;
        caculateDep(pRoot);
        return maxDepth;
        function caculateDep(root) {
            if (root.left) {
                count ++;
                caculateDep(root.left);
                root.left = null;
            }
            if (!root.left && root.right) {
                count ++;
                caculateDep(root.right);
                root.right = null;
            }
            if (!root.left && !root.right) {
                maxDepth = (count > maxDepth ? count : maxDepth);
                // 剪枝
                count --;
                root = null;
            }
        }
    }
}
```

Way2. 采用后序遍历的方式实现，在遍历某个左右子节点后，根据左右子结点的深度来判断这个树是不是平衡的，同时得到这个结点的深度。最后来确定这个树是不是平衡的。

```js

```

#### 58*. 数组中只出现一次的数字

题目：一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

解决方案：Way1. 借助hashMap，首先遍历一次数组，将数据映射到hashMap中去，然后将hashMap的键-值提取出来，进行一次遍历，得到符合结果的值

```js
function FindNumsAppearOnce(array)
{
    // write code here
    // return list, 比如[a,b]，其中ab是出现一次的两个数字
    var hashMap = {};
    for(let i = 0; i < array.length; i ++) {
        if (!hashMap[array[i]]) {
            hashMap[array[i]] = 1;
        }else {
            hashMap[array[i]] += 1;
        }
    }
    var keys = Object.keys(hashMap);
    var values = Object.values(hashMap);
    var results = [];
    for (let i = 0; i < values.length; i ++) {
        if (values[i] === 1) {
            results.push(parseInt(keys[i]))
        }
    }
    return results
}
```
但是该方法会占用辅助空间，空间复杂度较大
16ms， 5460K

Way2. 采用正则语句来实现，时间、空间复杂度均较高

```js
function FindNumsAppearOnce(array){
    if (!array || array.length < 2)
        return [];
    return array.sort().join(',').replace(/(\d+),\1/g,"").replace(/,+/g,',').replace(/^,|,$/, '').split(',').map(Number);
}
```

Way3. (最优)采用异或的机制来实现查找
1. 将数组中所有数字进行异或得到结果resXOR，resXOR等于只出现一次的两个数字的异或，原因：相同数字的异或为0，0与任何数的异或都等于那个数

2. 将resXOR用二进制表示，从低位到高位，找到第一次出现1的位置

3. 根据这个位置的值（0或者1），将原数组分为两类，这两个只出现一次的数字分别在其中一类中

4. 将两类中的数字分别做异或，即可得到这两个数字

```js
function FindNumsAppearOnce(array)
{
        // 首先将数组中所有的数求异或
    let xorRes = array.reduce((prev, cur) => prev ^ cur);
    let binaryRes = xorRes.toString(2);
    let firstOneIndex = -1;
    for (let i = binaryRes.length - 1; i >= 0; i --) {
        if (binaryRes[i] == 1) {
            firstOneIndex = binaryRes.length - i ;
            break;
        }
    }
    // 然后将原数组分为两个，并计算得到两个数值
    let res1, res2;
    for (let i = 0; i < array.length; i ++) {
        curBinary = array[i].toString(2);
        if (curBinary[curBinary.length - firstOneIndex] === '0') {
            // console.log("niubi")
            res1 = res1 ? (res1 ^ array[i]) : array[i];
        }else {
            res2 = res2 ? (res2 ^ array[i]) : array[i];
        }
    }
    return [res1, res2]
}
```

#### 59. 和为S的连续正整数序列

题目：小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列？输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

解决方案：设置两个指针，初始化一个为1，一个为2，然后根据叠加的和与sum值来进行判断，最终得到结果。

```js
function FindContinuousSequence(sum)
{
    // write code here
    let result = [];
    if (sum <= 1) {
        return result
    }
    let [firstPtr, lastPtr] = [1, 2];
    pieceSequence(firstPtr, lastPtr);
    return result
    
    function pieceSequence(ptr1, ptr2) {
        if (ptr2 === sum) {
            return;
        }
        let curSum = 0;
        for (let i = ptr1; i <= ptr2; i ++) {
            curSum += i;
        }
        // curSum大于sum，first向右移动指针
        if (curSum > sum) {
            pieceSequence(++ptr1, ptr2);
        }
        if (curSum < sum) {
            pieceSequence(ptr1, ++ptr2);
        }
        if (curSum === sum) {
            let curResult = [];
            for (let i = ptr1; i <= ptr2; i ++) {
                curResult.push(i)
            }
            result.push(curResult)
            pieceSequence(++ptr1, ptr2);
        }
    }
}
```

#### 60. 和为S的两个数字

题目：输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

```js
function FindNumbersWithSum(array, sum)
{
    // write code here
    if (array.length < 2) {
        return []
    }
    let [curr1, curr2] = [0, 1];
    for (let i = curr1; i < array.length; i ++) {
        for (let j = curr2; j < array.length; j ++) {
            if (array[i] + array[j] === sum) {
                return [array[i], array[j]]
            }
        }
    }
    return []
}
```

#### 61. 翻转单词顺序列

题目：牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

解决方案：在JS中采用正则的方式提取，然后使用reverse方法翻转数组，之后合并为字符串即可

```js
function ReverseSentence(str)
{
    return str.split(" ").reverse().join(" ")
}
```

#### 62. 左旋转字符串

题目：汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

解决方案：题目的关键在于循环左移，因此需要进行一次求余的计算

```js
function LeftRotateString(str, n)
{
    // write code here
    if(!str){
        return "";
    }
    var len = str.length;
    n = n % len;
    var left = str.slice(0, n);
    var right = str.slice(n);
    return right + left;
}
```

#### 63. 滑动窗口的最大值

题目：给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

解决方案：设置一个两端开口的队列，最大值永远在第一位，并对进来的数字进行判断。如果进来的数字大于最大的数字，则进行替换；如果进来的数字小于队列的数字，则进行保存。同时需要注意窗口长度。

```js
function maxInWindows(num, size)
{
    var len = num.length
    if (size > len || size === 0) {
        return []
    }
    var [results, curSeq] = [[], [0]];
    for (let i = 0; i < len; i ++) {
        let flag = 0;
        for (let j = 0; j < curSeq.length; j ++) {
            if (num[i] >= num[curSeq[j]]) {
                curSeq.splice(j, curSeq.length - j, i);
                flag = 1;
                break
            }
        }
        if (flag === 0) {
            curSeq.push(i)
        }
        // 删除超过窗口宽度的数字
        if (curSeq[curSeq.length - 1] - curSeq[0] >= size) {
            curSeq.shift();
        }
        if (i + 1 >= size) {
            results.push(num[curSeq[0]])
        }
    }
    return results
}
```

#### 64. 扑克牌中的顺子

题目： LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

解决方案：1. 首先将数组进行排序
2. 统计数组中0的个数
3. 统计排序后的数组中，相邻的数字之间的空缺数
4. 如果数组中出现重复数字的情况，则认定为不能组成顺子

```js
function IsContinuous(numbers)
{
    // 错误输入的情况
    var len = numbers.length;
    if (len < 1 || !numbers) {
        return false 
    }
    // Step1. 排序数组
    sortedArr = numbers.sort();
    // Step2. 统计0的个数
    // Step3. 判断有没有重复的数字
    var zeroNum = 0;
    var diffNum = 0;
    for (let i = 0; i < len; i ++) {
        if (sortedArr[i] === 0) {
            zeroNum ++;
        }
        if (sortedArr[i + 1] && sortedArr[i] === sortedArr[i + 1] && sortedArr[i] !== 0) {
            return false
        }
        if (sortedArr[i + 1] && sortedArr[i + 1] - sortedArr[i] !== 1 && sortedArr[i] !== 0) {
            diffNum += (sortedArr[i + 1] - sortedArr[i] - 1);
        }
    }
    return zeroNum >= diffNum ? true : false 
}
```

#### 65. 圆圈中最后剩下的数字

题目：每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

解决方案：映射的方程为p(x)=(x-k-1)%n，然后得到递归的公式

```
f(n, m) = (f(n-1, m) + m) % n
```

```js
function LastRemaining_Solution(n, m)
{
    // write code here
    if (n < 1 || m < 1) {
        return -1
    }
    var last = 0;
    for (let i = 2; i <= n; i ++) {
        last = (last + m) % i
    }
    return last
}
```

#### 66. 股票的最大利润

题目：股票的价格按照时间先后存储在数组，卖该股票一次最大的利润是多少？

解决方案：卖出价固定时，买入价越低则利润越大。需要记住之前i-1个数字的最小值，就可以计算最大利润。

```js
function maxProfit(array) {
    if (!array || array.length === 0) {
        return 0
    }
    let lowest = array[0];
    let maxProfit = 0;
    for (let i = 0; i < array.length; i ++){
        lowest = array[i] < lowest ? array[i] : lowest;
        if (array[i] - lowest > maxProfit) {
            maxProfit = array[i] - lowest;
        }
    }
    return maxProfit
}
```

#### 67. 求1+2+...+n

题目：求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）

解决方案：

```js
function Sum_Solution(n){
    var sum = 0;
    plus(n);
    return sum;
     
    function plus(num){
        sum += num;
        num > 0 && plus(--num);
    }
}
```

#### 68. 不用加减乘除做加法

题目：
写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

解决方案：加法的步骤可以理解为以下几个步骤：

1. 只做各位相加不进位（异或）

2. 做进位，如果有进位则进行进位（两个数先做位与运算，然后左移一位）

3. 将前两个步骤的结果相加（也就是重复前面两个步骤，直到不产生进位）

```js
function Add(num1, num2)
{
    // write code here
    return addCore(num1, num2)

    function addCore(bool1, bool2) {
        // Step1: 进行不进位的加法运算，也就是异或运算
        xorRes = bool1 ^ bool2;
        // Step2: 产生进位，俩个数进行与运算，然后进行移位
        andRes = bool1 & bool2;
        if (andRes !== 0) {
            andRes = andRes << 1
            addCore(xorRes, andRes)
        }
        return xorRes
    }
}
```

#### 69. 构建乘积数组

题目：给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]\*A[1]\*...\*A[i-1]\*A[i+1]\*...\*A[n-1]。不能使用除法。

解决方案：B[i]=A[0]\*A[1]\*...\*A[i-1]\*A[i+1]\*...\*A[n-1]可以看作两个部分C[i]=A[0]\*A[1]\*...\*A[i-1]和D[i]=A[i+1]\*...\*A[n-1]两个部分。可以通过自上而下和自下而上两个顺序计算得到。

```js
function multiply(array)
{
    // write code here
    var len = array.length;
    var bArr = [];
    var cArr = [1];
    var dArr = [1];
    for (let i = 1; i < len; i ++) {
        cArr.push(cArr[i - 1] * array[i - 1]);
        dArr.push(dArr[i - 1] * array[array.length - i])
    }
    for (let i = 0; i < len; i ++) {
        bArr.push(cArr[i] * dArr[len-i-1])
    }
    return bArr
}
```

#### 70. 把字符串转换成整数

题目：将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

解决方案：判断多种违规输入的情况，也需要考虑正负的情况。

```js
function StrToInt(str)
{
    var len = str.length;
    if (len <= 0) {
        return 0
    }
    let numbers = 0;
    let sign = str[0] === '-' ? -1 : 1;
    for (let i = (str[0] === '+' || str[0] === '-') ? 1 : 0; i < len; i ++) {
        if (str[i] < '0' || str[i] > '9') {
            return 0
        }
        numbers = numbers * 10 + parseInt(str[i])
    }
    return numbers * sign
}
```

#### 71*. 字符流中第一个不重复的字符

题目：请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

```js
function Init(){
    streamNums = [];   //定义一个全局变量, 不用var
    streamNumsLen = 256;   //定义一个全局变量, 不用var
    streamNumsIndex = 0;   //定义一个全局变量, 不用var
    for(var i = 0; i < streamNumsLen; i++){
        streamNums[i] = -1;
    }
}
function Insert(ch){
    var code = ch.charCodeAt();
    if(streamNums[code] == -1){
        streamNums[code] = streamNumsIndex;
    } else if(streamNums[code] >= 0){
        streamNums[code] = -2;
    }
    streamNumsIndex++;
}
function FirstAppearingOnce(){
    result = '';
    var ch = '';
    var minIndex = Infinity;
    for(var i = 0; i < streamNumsLen; i++){
        if(streamNums[i] >= 0 && streamNums[i] < minIndex){
            ch = String.fromCharCode(i);
            minIndex = streamNums[i];
        }
    }
    return ch == "" ? '#' : ch;
}
```

#### 72*. 按之字形顺序打印二叉树

题目：请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

```js
function zPrint(pRoot) {
   // write code here
    let res = [];
    if (!pRoot) {
        return res
    }
    let count = 1;
    let queue = [pRoot];
    zPrintCore(queue, count);
    return res
    function zPrintCore(queCen, count) {
        let resCen = [];
        let len = queCen.length;
        let flag = 0;
        for (let i = 0; i < len; i ++) {
            flag += Boolean(queCen[i])
        }
        if (flag) {
            if (count % 2 === 1) {
                // 奇数层顺序打印
                for (let i = 0; i < len; i ++) {
                    if (queCen[i]){
                        resCen.push(queCen[i].val);
                    }
                }
                // 更新queCen
                for (let i = len - 1; i >= 0; i --) {
                    if (queCen[i].right){
                        queCen.push(queCen[i].right)
                    }
                    if (queCen[i].left) {
                        queCen.push(queCen[i].left)
                    }
                }
                queCen.splice(0, len)
                res.push(resCen);
                count ++;
                zPrintCore(queCen, count);
            }else {
                // 偶数层逆序打印
                for (let i = 0; i < len; i ++) {
                    if (queCen[i]){
                        resCen.push(queCen[i].val);
                    }
                }
                // 更新queCen
                for (let i = len - 1; i >= 0; i --) {
                    if (queCen[i].left){
                        queCen.push(queCen[i].left)
                    }
                    if (queCen[i].right) {
                        queCen.push(queCen[i].right)
                    }
                }
                queCen.splice(0, len);
                res.push(resCen);
                count ++;
                zPrintCore(queCen, count);
            }
        }else {
            return;
        }
    } 
}
```

#### 73. 把二叉树打印成多行

题目：从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

```js
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function Print(pRoot)
{
    var res = [];
    if(!pRoot){
        return res;
    }
    var que = [];
    que.push(pRoot);
    while(que.length > 0){
        var vec = [];
        var len = que.length;
        for(var i = 0; i < len; i++){
            var tmp = que.shift(); //front
            vec.push(tmp.val);
            if(tmp.left)
                que.push(tmp.left);
            if(tmp.right)
                que.push(tmp.right);
        }
        res.push(vec);
    }
    return res;
}
```