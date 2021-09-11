```python
class Solution:
    def check(self, str1):
        i = 0
        length = len(str1)
        if length == 0: return True
        if length % 2 == 1:
            while True:
                if i >= length / 2: return True
                if str1[i] != str1[length - 1 - i]:
                    return False
                i += 1
        else:
            while True:
                if i >= length / 2 - 1: return True
                if str1[i] != str1[length - 1 - i]:
                    return False
                i += 1

            
    def canBePalindromicString(self , str1 ):
        # write code here
        if not str1: return 0
        if self.check(str1): return 1
        l, r = 0, len(str1) - 1
        while l < r:
            if str1[l] != str1[r]:
                return self.check(str1[l+2:r+1]) or self.check(str1[l:r-1])
            l += 1
            r -= 1
        return 1
```
