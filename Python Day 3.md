
# Ascii -> Decimal -> Binary

  ## Ordinal number:
  ### ord() -- gives decimal equivalant of any single character string --

		NukeCodes = 'a'
		ord(NukeCodes)
		97


  # Binary Conversion:

## Converts Decimal to binary - 
		bin(ord(NukeCodes))
        	'0b1100001'

       *** Prefered Method***

## Format Binary and pads leading zero in conversion
		format(ord(NukeCodes), '0>8b')
		'01100001'

## Binary -> Decimal -> Ascii

### Convert Binary -> Decimal 
   		int('01100001', 2)

### Decimal -> Single Character String
		chr(97)


## Pe1/part 2

		  #!/usr/bin/env python3
		   
		  def steg_encode_char(char, cover):
		       '''LSB encodes a character
		       Args:
		           char (str): a single character string
		           cover (list): list of 8 strings representing integers in the range [0-255]
		       Returns:
		           None
		      '''
		      binChar = format(ord(char), '0>8b')
		      binCharList = list(binChar)
		  
		      for i in range(0,8):
		          coverbin = format(int(cover[i]),'0>8b')
		          coverbinList = list(coverbin)
		          coverbinList[-1] =  binCharList[i]
		          newCover = ''.join(coverbinList)
		          cover[i] = str(int(newCover, 2))
		  
		      
		pass
		  
		  def steg_decode_char(stego):
		      '''LSB decodes a character
		      Args:
		          stego (list): list of 8 strings representing integers in the range [0-255]
		      Returns:
		          str: character that was decoded
		      '''
		      characterL = []
		      for i in range(0,8):
		          stegoList = list(format(int(stego[i]), '0>8b'))
		          characterL.append(stegoList[-1])
		      character = chr(int(''.join(characterL),2))
		      return character
		  
		  if __name__ == '__main__':
		      pass


# --FILE IO--
## 'r' read (default)
## 'w' write

## Opening and Closing a file:
  ## with statement 
  
		with open('myfile.txt', 'r') as fp:
			fp.read()
		  	fp.readline()
		  	fp.readlines()

		with open('test.txt', 'w') as fp:
		    fp.write('First line\n')
		    lines = ['Second line\n', 'Third line\n', 'Fourth line\n', 'Last line\n']
		    fp.writelines(lines)

  
with open("test.txt") as source, open("copy.txt", 'w') as destination:
    destination.write(source.read())


SETS:
Cant pull items out of a set
only check if it is present.
s = {1,2,3,4,5}
s.add(100)
s.add(0)
s.add(99)
print(s)
{0, 1, 2, 3, 4, 5, 100, 99}
1 in s
True
50 in s
False

DICTIONARIES:
key: value pairs

romanNumerals = {'I':1, 'V':5, 'X':10, 'L':50}

romanNumerals['X']
10

Adding to a dictionary
romanNumerals['C'] = 100
romanNumerals['D'] = 500
romanNumerals['M'] = 1000

romanNumerals['C']
100

deleting from a dictionary
del romanNumerals['C']
'C' in romanNumerals
'M' in romanNumerals

---Returns keys---
for i in romanNumerals:
  print(i)

---Return Values---
for i in romanNumerals:
  	print(romanNumerals[i])

****Testing to see how many of each character are in the string using Dictionaries****

myStr = "According to all known laws of aviation, the bee should not be able to fly"
myDict = {}
for i in my Str:
    if i in myDict:
        myDict[i] += 1
    else:
        myDict[i] = 1


VARIABLE LENGTH ARGUMENT:
def myFunction(*args):
	product = 1
	for i in args:
		product = product * i
	return product
print(myFunction(5,10,50,100))

map function 
does the function on left to every item iterable on the right

		map(function, list)
		myList = [2, 3, 4, 5]
		print(list(map(lamda x:x**2, myList)))
all()
determines if entire list is true

/////////////////////////////////////////////////////////////////////////////////////

Challenge: Using the file school_prompt.txt, if the character ‘p’ is in a word, then add the word to a list called p_words.

with open('school_prompt.txt', 'r') as fb:
    p_words = []

    words = fb.read().split()
    for word in words:
        if 'p' in word:
            p_words.append(word)
        else: 
            continue
            
# p_words = [i for i in fb.read().split() if 'p' in i]





