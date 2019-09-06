1. 排序问题
  ```js
    //公共函数
    function swap(ary, i, j) {
      let tmp = ary[i];
      ary[i] = ary[j];
      ary[j] = tmp;
    }
    
    //冒泡排序
    function bubbleSort(ary) {
      for (let i = ary.length - 2; i >= 0; i--) {
        let exchange = false;
        for (let j = 0; j <= i; j++) {
          if (ary[j] > ary[j + 1]) {
            swap(ary, j, j + 1);
            exchange = true;
          }
        }
        if (!exchange) {
          return ary;
        }
      }
      return ary;
    }
    
    //选择排序
    function selectSort(ary) {
      for (let i = 0; i < ary.length - 1; i++) {
        let minIndex = i;
        for (let j = i + 1; j < ary.length; j++) {
          if (ary[j] < ary[minIndex]) {
            minIndex = j;
          }
        }
        if (minIndex !== i) {
          swap(ary, i, minIndex);
        }
      }
      return ary;
    }

    //归并排序
    function merge(ary) {
      if (ary.length < 2) {
        return ary.slice();
      }
      let mid = Math.floor(ary.length / 2);
      let leftAry = ary.slice(0, mid);
      let rightAry = ary.slice(mid);
      let leftSort = merge(leftAry);
      let rightSort = merge(rightAry);
      let res = [];
      while (leftSort.length > 0 && rightAry.length > 0) {
        if (leftSort[0] < rightSort[0]) {
          res.push(leftSort.shift());
        } else {
          res.push(rightSort.shift());
        }
      }
      res.push(...leftSort, ...rightSort);
      return res;
    }

    //快速排序
    function quickSort(ary, left = 0, right = ary.length - 1) {
      if (right - left <= 0) {
        return
      }
      let baseIndex = Math.floor(Math.random() * (right - left)) + left;
      let base = ary[baseIndex];
      swap(ary, baseIndex, right);
      let j = left;
      for (let i = left; i <= right; i++) {
        if (ary[i] < base) {
          swap(ary, i, j);
          j++;
        }
      }
      swap(ary, j, right);
      quickSort(ary, left, j - 1);
      quickSort(ary, j + 1, right);
      return ary;
    }

    //BST排序
    function insertIntoBST(bst, value) {
      if (!bst) {
        return {
          val: value,
          left: null,
          right: null
        }
      }
      if (value < bst.val) {
        bst.left = insertIntoBST(bst.left, value);
      } else {
        bst.right = insertIntoBST(bst.right, value);
      }
      return bst;
    }

    function inOrderTraverse(bst, action) {
      if (bst) {
        inOrderTraverse(bst.left, action);
        action(bst.val);
        inOrderTraverse(bst.right, action);
      }
    }

    function bstSort(ary) {
      if (ary.length < 2) {
        return ary.slice();
      }
      let tree = ary.reduce(insertIntoBST, null);
      let res = [];
      inOrderTraverse(tree, function(val) {res.push(val)});
      return res;
    }
  ```

2. 两数之和
  ```js
    function twoSum(ary, target) {
       let map = {};
       for (let i = 0; i < ary.length; i++) {
         let cur = ary[i];
         if (map[target - cur] != undefined) {
           return [ary[map[target - cur]], cur];
         }
         map[cur] = i;
       }
    }
  ```

3. 斐波那契数列(爬楼梯问题)
  ```js
    function fibo(n, cache={}) {
      if (n === 1) {
        return 1;
      }
      if (n === 2) {
        return 2;
      }
      if (cache[n] != undefined) {
        return cache[n];
      }
      cache[n] = fibo(n - 1, cache) + fibo(n - 2, cache);
      return cache[n];
    }
  ```

4. 最大子序列和问题
  ```js
    //复杂度为 O(N^2)
    function maxSubAry(ary) {
      let max = -Infinity;
      for (let i = 0; i < ary.length; i++) {
        let sum = 0;
        for (let j = i; j < ary.length; j++) {
          sum += ary[j];
          if (sum > max) {
            max = sum;
          }
        }
      }
      return max;
    }

    //复杂度为 O(N)
    function maxSubAry(ary) {
      let max = -Infinity;
      let sum = 0;
      for (let i = 0; i < ary.length; i++) {
        sum += ary[i];
        if (sum > max) {
          max = sum;
        }
        if (sum < 0) {
          sum = 0;
        }
      }
      return max;
    }

    //分治法
    function maxSubAry(ary) {
      let len = ary.length;
      if (len === 1) {
        return ary[0];
      }
      let mid = Math.floor(len / 2);
      let leftAry = ary.slice(0, mid);
      let rightAry = ary.slice(mid);
      let maxLeft = maxSubAry(leftAry);
      let maxRight = maxSubAry(rightAry);
      let maxMid;
      let maxMidLeft = -Infinity;
      let maxMidRight = -Infinity;
      let tmpLeft = 0;
      let tmpRight = 0;
      for (let i = mid - 1; i >= 0; i--) {
        tmpLeft += ary[i];
        if (tmpLeft > maxMidLeft) {
          maxMidLeft = tmpLeft;
        }
      }
      for (let j = mid; j < len; j++) {
        tmpRight += ary[j];
        if (tmpRight > maxMidRight) {
          maxMidRight = tmpRight;
        }
      }
      maxMid = maxMidLeft + maxMidRight;
      return Math.max(maxLeft, maxRight, maxMid);
    }
  ```

5. 无重复字符的最长子串
  ```js
    function lengthOfLongestSubstring(ary) {
      let len = ary.length;
      let j = 0, max = 0;
      let map = {};
      for (let i = 0; i < len; i++) {
        let cur = ary[i];
        if (map[cur] != undefined) {
          j = Math.max(j, map[cur] + 1);
        }
        map[cur] = i;
        max = Math.max(max, i - j + 1);
      }
      return max;
    }
  ```

6. 捡金币问题(金币呈三角形分布)
  ```js
    var moneys = [
      [2],
      [3, 4],
      [6, 5, 7],
      [4, 1, 8, 3]
    ];

    function maxMoney(x, y, cache={}) {
      let key = x + ',' + y;
      if (key in cache) {
        return cache[key];
      }
      if (x === moneys.length - 1) {
        return moneys[x][y];
      } else {
        return cache[key] = moneys[x][y] + Math.max(maxMoney(x + 1, y, cache), maxMoney(x + 1, y + 1, cache));
      }
    }
  ```

7. 链表反转
  ```js
    //递归
    function reverseList(head) {
      if (!head || !head.next) {
        return head;
      }
      let newHead = reverseList(head.next);
      head.next.next = head;
      head.next = null;
      return newHead;
    }

    //循环
    function reverseList(head) {
      if (!head || !head.next) {
        return head;
      }
      let a = null;
      let b = null;
      let c = head;
      while (c) {
        a = b;
        b = c;
        c = c.next;
        b.next = a;
      }
      return b;
    }
  ```

8. 实现 indexOf
  ```js
    function indexOf(s1, s2) {
      let l1 = s1.length;
      let l2 = s2.length;
      let t = 0;
      if (l2 === 0) {
        return 0;
      }
      for (let i = 0; i < l1; i++) {
        let cur = s1[i];
        if (cur === s2[t]) {
          t++;
        } else {
          i = i - t;
          t = 0;
        }
        if (t === l2) {
          return i - t + 1;
        }
      }
      return -1;
    }
  ```

9. 有序数组的二分查找
  ```js
    //递归
    function binarySearch(target, arr, start, end) {
      let start = start || 0;
      let end = end || arr.length - 1;
      let mid = Math.floor((start + end) / 2);
      if (start > end) {
        return -1;
      }
      if (arr[mid] === target) {
        return mid;
      } else if (arr[mid] > target) {
        return binarySearch(target, arr, start, mid - 1);
      } else {
        return binarySearch(target, arr, mid + 1, end);
      }
    }

    //循环
    function binarySearch(target, arr, start, end) {
      let start = start || 0;
      let end = end || arr.length - 1;
      let mid;
      while (start <= end) {
        mid = Math.floor((start + end) / 2);
        if (arr[mid] === target) {
          return mid;
        } else if (arr[mid] > target) {
          end = mid - 1;
        } else {
          start = mid + 1;
        }
      }
      return -1;
    }
  ```