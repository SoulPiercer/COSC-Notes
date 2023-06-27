# Practice Test
## public/activities/practice-exam/v1

### Question 1
    def q1(floatstr):
      '''
      TLO: 112-SCRPY002, LSA 3,4
      Given the floatstr, which is a comma separated string of
      floats, return a list with each of the floats in the 
      argument as elements in the list.
      '''
      
      newlist = []
      flt = floatstr.split(',')
      for item in flt:
        newlist.append(float(item))
        return newlist
        pass

      ## one Liner
      return list(map(float, floatstr.split(',')))
### Question 2
## one liner
    def q2(*args):
        return (sum(args)/len(args))
    
    def q2(*args):
        '''
        TLO: 112-SCRPY006, LSA 3
        TLO: 112-SCRPY007, LSA 4
        Given the variable length argument list, return the average
        of all the arguments as a float
        '''
        average = 0
        counter = 0
        for item in args:
            average += int(item)
            counter += 1
        return float(average/counter)
  
        

### Question 3
    def q3(lst,n):
        return lst[-n:]
      '''
        TLO: 112-SCRPY004, LSA 3
        Given a list (lst) and a number of items (n), return a new 
        list containing the last n entries in lst.
        '''
        newlst = []
        i = -n
        x = n
        while i < 0:
          newlst.append(lst[i])
          i += 1
        return newlst
        pass

### Question 4
        def q4(strng):
            return list(map(ord, strng))
            
            '''
            TLO: 112-SCRPY004, LSA 1,2
            TLO: 112-SCRPY006, LSA 3
            Given an input string, return a list containing the ordinal numbers of 
            each character in the string in the order found in the input string.
            '''
            newlst = []
            strlist = list(strng)
            for i in strlist:
              newlst.append(ord(i))
            return newlst
            pass

### Question 5
        def q5(strng):
              '''
              TLO: 112-SCRPY002, LSA 1,3
              TLO: 112-SCRPY004, LSA 2
              Given an input string, return a tuple with each element in the tuple
              containing a single word from the input string in order.
              '''
              lst = tuple(strng.split())
              return lst
              pass

### Question 6
        def q6(catalog, order):
            '''
            TLO: 112-SCRPY007, LSA 2
            Given a dictionary (catalog) whose keys are product names and values are product
            prices per unit and a list of tuples (order) of product names and quantities,
            compute and return the total value of the order.
            
            Example catalog:
            {
              'AMD Ryzen 5 5600X': 289.99,
              'Intel Core i9-9900K': 363.50,
              'AMD Ryzen 9 5900X': 569.99
            }
            
            Example order:
            [
              ('AMD Ryzen 5 5600X', 5), 
              ('Intel Core i9-9900K', 3)
            ]
            
            Example result:
            2540.45 
            
            How the above result was computed:
            (289.99 * 5) + (363.50 * 3)
            '''
            total = 0
            for item in order:
              total += catalog[item[0]] * item[1]
            
            return total
            pass

### Question 7
        def q7(filename):
        '''
        TLO: 112-SCRPY005, LSA 1
        Given a filename, open the file and return the length of the first line 
        in the file excluding the line terminator.
        '''
    
            with open(filename) as fb1:
                return len(fp.readlines()[0]) -1

                
### Question 8
        def q8(filename,lst):
            '''
            TLO: 112-SCRPY003, LSA 1
             TLO: 112-SCRPY004, LSA 1,2
         
            TLO: 112-SCRPY005, LSA 1
            Given a filename and a list, write each entry from the list to the file
            on separate lines until a case-insensitive entry of "stop" is found in 
            the list. If "stop" is not found in the list, write the entire list to 
            the file on separate lines.
            '''
        
            with open(filename,'w') as fb1:
        
                for item in lst:
                    if item.lower() != 'stop':
                        fb1.write(item +'\n')
                    elif item.lower() == 'stop':
                        break
                return fb1
    ## If writing a list, use wrtielines()
    ## if writing a string, use write()


### Question 9
        def q9(miltime):
            '''
            TLO: 112-SCRPY003, LSA 1
            Given the military time in the argument miltime, return a string 
            containing the greeting of the day.
            0300-1159 "Good Morning"
            1200-1559 "Good Afternoon"
            1600-2059 "Good Evening"
            2100-0259 "Good Night"
            '''
        
            if miltime in range(300,1200):
                return "Good Morning"
            elif miltime in range(1200,1600):
                return "Good Afternoon"
            elif miltime in range(1600,2100):
                return "Good Evening"
            elif miltime in range(2100,2400) or miltime in range(000,300):
                return "Good Night"
            else:
                return None
         pass

 ### Question 10
        def q10(numlist):

            # return all(map(lambda x: x > 0, numlist))
            '''
            TLO: 112-SCRPY003, LSA 1
            TLO: 112-SCRPY004, LSA 1
            Given the argument numlist as a list of numbers, return True if all 
            numbers in the list are NOT negative. If any numbers in the list are
            negative, return False.
            '''
            for number in numlist:
                if number < 0:
                    return False
                else:
                    continue
            return True

## practice-exam/v2

### Question 1
        def q1(sentence):
        '''
        Given a string of multiple words separated by single spaces,
        return a new string with the sentence reversed. The words
        themselves should remain as they are. For example, given
        'it is accepted as a masterpiece on strategy', the returned
        string should be 'strategy on masterpiece a as accepted is it'.
        '''
        return ' '.join(sentence.split()[::-1])

 ### Question 2
 
        def q2(n):
            '''
            Given a positive integer, return its string representation with
            commas seperating groups of 3 digits. For example, given 65535
            the returned string should be '65,535'.
            '''
            return (f"{n:,}")
      
      
 ### Question 3
 
        def q3(lst0, lst1):
            '''
            Given two lists of integers, return a sorted list that contains
            all integers from both lists in descending order. For example,
            given [3,4,9] and [8,1,5] the returned list should be [9,8,5,4,3,1].
            The returned list may contain duplicates.
            '''
            lst = []
            for i in lst0:
                lst.append(i)
            for i in lst1:
                lst.append(i)
            return sorted(lst, reverse = True)

### Question 4
        def q4(s1,s2,s3):
            '''
            Given 3 scores in the range [0-100] inclusive, return 'GO' if
            the average score is greater than 50. Otherwise return 'NOGO'.
            '''
        
            if (s1+s2+s3/3) > 50:
                return "GO"
            else:
                return "NOGO"
        

### Question 5

        def q5(integer, limit):
            '''
            Given an integer and limit, return a list of even multiples of the
            integer up to and including the limit. For example, if integer==3 and
            limit==30, the returned list should be [0,6,12,18,24,30]. Note, 0 is
            a multiple of any integer except 0 itself.
            '''
            """
            evenSteps = []
            for number in range(0,limit +1,integer):
                if number%2 == 0:
                    evenSteps.append(number)
            return evenSteps
        """
        
            return [number for number in range(0,(limit +1),integer) if number%2 == 0]

### Question 6
        def q6(f0, f1):
            '''
            Given two filenames, return a list whose elements consist of line numbers
            for which the two files differ. The first line is considered line 0.
            '''
            difLines = []
            with open(f0) as file0, open(f1) as file1:
                lst0 = file0.readlines()
                lst1 = file1.readlines()
                for i in range(0,len(lst0)):
                    if lst0[i] != lst1[i]:
                        difLines.append(i)
                        """
                for i in range(0,len(lst1)):
                    if lst1[i] != lst0[i]:
                        difLines.append(i)
                        """
            return difLines

### Question 7
        def q7(lst):
            '''
            Return the first duplicate value in the given list.
            For example, if given [5,7,9,1,3,7,9,5], the returned value should
            be 7.
            '''
            unique = {}
            for i in lst:
                if i in unique:
                    return i
                else:
                    unique[i] = 1
     

### Question 8

        def q8(strng):
            '''
            Given a sentence as a string with words being separated by a single space,
            return the length of the shortest word.
            '''
            return len(min(strng.split(), key=len))

### Question 9
        def q9(strng):
            '''
            Given an alphanumeric string, return the character whose ascii value
            is that of the integer represenation of all of the digits in the string
            concatenated in the order in which they appear. For example, given
            'hell9oworld7', the returned character should be 'a' which has
            the ascii value of 97.
            '''
            number = []
            for i in strng:
                if i >= '0' and i <= '9':
                    number += i
            integer = (''.join(number))
            return chr(int(integer))
     
    
### Question 10

        def q10(arr):
            '''
            Given a list of positive integers sorted in ascending order, return
            the first non-consecutive value. If all values are consecutive, return
            None. For example, given [1,2,3,4,6,7], the returned value should be 6. 
            '''
            for i in range(0,len(arr)):
                if i > arr[i]-1 and i < arr[i] +1:
                    continue
                else:
                    return arr[i]


                
     

            
