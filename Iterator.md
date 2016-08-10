## SGI版本STL迭代器

-------------------------------------------------------------------

<br>

```cpp

		#pragma once

		//
		// 迭代器的型别
		//
		struct InputIteratorTag {};
		struct OutputIteratorTag {};
		struct ForwardIteratorTag : public InputIteratorTag {};
		struct BidirectionalIteratorTag : public ForwardIteratorTag {};
		struct RandomAccessIteratorTag : public BidirectionalIteratorTag {};
		
		//
		// Traits 就像一台“特性萃取机”，榨取各个迭代器的特性（对应的型别）
		//
		template <class Iterator>
		struct IteratorTraits
		{
			typedef typename Iterator:: IteratorCategory IteratorCategory ;
			typedef typename Iterator:: ValueType        ValueType;
			typedef typename Iterator:: DifferenceType   DifferenceType ;
			typedef typename Iterator:: Pointer          Pointer;
			typedef typename Iterator:: Reference        Reference;
		};
		
		template <class T>
		struct IteratorTraits<T*>
		{
			typedef typename RandomAccessIteratorTag IteratorCategory;
			typedef typename T			 ValueType;
			typedef typename ptrdiff_t   DifferenceType ;
			typedef typename T*          Pointer;
			typedef typename T&			 Reference;
		};
		
		template <class T>
		struct IteratorTraits<const T*>
		{
			typedef typename RandomAccessIteratorTag IteratorCategory ;
			typedef typename T			 ValueType;
			typedef typename ptrdiff_t   DifferenceType ;
			typedef typename const T*    Pointer;
			typedef typename const T&	 Reference;
		};
		
		// O(N)
		template <class InputIterator>
		inline size_t __Distance
		(InputIterator first, InputIterator last, InputIteratorTag) {
			size_t n = 0;
			while (first != last)
			{
				++first;
				++n;
			}
		
			return n;
		}
		
		// O(1)
		template <class InputIterator>
		inline size_t __Distance
		(InputIterator first, InputIterator last, RandomAccessIteratorTag) {
			return last - first;
		}
		
		//template <class InputIterator, class Distance>
		//inline void Distance
		//(InputIterator first, InputIterator last, Distance& n)
		//{
		//	n = __Distance(first, last,
		//		IteratorTraits<InputIterator>::IteratorCategory());
		//}
		
		template <class InputIterator>
		inline size_t Distance
		(InputIterator first, InputIterator last)
		{
		
			//return __Distance(first, last,
			//	IteratorTraits<InputIterator>::IteratorCategory());
		
			return __Distance(first, last,
				InputIterator::IteratorCategory());
		}
		



```
