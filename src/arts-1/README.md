# Algorithm
> https://leetcode.com/problems/find-common-characters/

自己的解题思路：
```java
class Solution {
    public List<String> commonChars(String[] A) {
		Map<Character, byte[]> temp = new HashMap<>();
		for (byte i = 0; i < A.length; i++) {
			char[] chars = A[i].toCharArray();
			for (char aChar : chars) {
				byte[] bytes = temp.get(aChar);
				if (bytes == null) {
					bytes = new byte[A.length];
				}
				bytes[i] = ++bytes[i];
				temp.put(aChar, bytes);
			}
		}
		List<String> result = new ArrayList<>(temp.size());
		index: for (Map.Entry<Character, byte[]> characterEntry : temp.entrySet()) {
			byte[] value = characterEntry.getValue();
			byte size = value[0];
			for (byte i = 1; i < value.length; i++) {
				size = (size > value[i]) ? value[i] : size;
			}
			if (size == 0) {
				continue index;
			}
			for (byte j = 0; j < size; j++) {
				result.add(String.valueOf(characterEntry.getKey()));
			}
		}
        return result;
    }
}
```

别人的思路
```java
class Solution {
    public List<String> commonChars(String[] A) {
      int[] all_dict = new int[26];
    List<String> result = new ArrayList<String>();
    for (int i = 0; i < 26; i ++) {
        all_dict[i] = Integer.MAX_VALUE;
    }
    for (String word: A) {
        int[] dict = new int[26];
        for (int i = 0; i < 26; i ++) {
            dict[i] = 0;
        }
        
        for (Character c: word.toCharArray()) {
            dict[c - 'a'] ++;
        }
        for (int k = 0; k < 26; k ++) {
            if (dict[k] < all_dict[k]) {
                all_dict[k] = dict[k];
            }
        }
    }
    for (int k = 0; k < 26; k ++) {
        if ((all_dict[k] != 0) && (all_dict[k] != Integer.MAX_VALUE)) {
            for (int i = 0; i < all_dict[k]; i ++) {
                result.add(Character.toString((char)(k+'a')));
            }
        }
    }
    return result;
    }
}
```

# Review
[Request’s Past, Present and Future](https://github.com/request/request/issues/3142)

request一开始占用很大的市场。
随着Js不断迭代，request进行各种升级，
当版本改动越来越大且无法向下兼容时，作者考虑停止维护，给没有历史包袱的新框架让路。


# Tips
[linux vim用法](./vim.md)

# Share
[分布式事务](./distributed_transaction.md)