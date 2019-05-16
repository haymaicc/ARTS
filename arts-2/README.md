### Algorithm
> https://leetcode.com/problems/valid-parentheses/description

```java
class Solution {
    public boolean isValid(String s) {
        Stack stack = new Stack<Character>();
	Map<Character, Character> mappers = new HashMap<>(3);
	mappers.put('{', '}');
	mappers.put('[', ']');
	mappers.put('(', ')');
	for (int i = 0; i < s.length(); i++) {
		char c = s.charAt(i);
		if ("{[(".indexOf(c) != -1) {
			stack.push(c);
		} else {
	if(stack.isEmpty()){
	    return false;
	}
	char pop =  mappers.get(stack.pop());
	if(pop != c){
	    return false;
	}
		}
	}
	return stack.isEmpty();
    }
}
```

### Review
> https://blog.kowalczyk.info/article/19f2fe97f06a47c3b1f118fd06851fad/lessons-learned-porting-50k-loc-from-java-to-go.html

### Tips
> 

### Share
