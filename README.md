# jounral entry 5
## subheading
Contiguous Allocation:


Its fixed size of a file in which the file cannot go beyond. 


Linked List Allocation:

For Linked list allocation  each file block is on a disk that is associated with a pointer to the next block.
The most notable example is the MS DOS file system, which uses the file attribute table (FAT) to implement linked list files.

When a file header points to the table entry of the first file block, and the content of the file block contains the entry number of the next block.  A special marker is then used to indicate the end of that particular  file.

One of  the  advantages of the linked list  approach is that the files can grow  dynamically with incremental allocation of blocks.  However, sequential accesses may suffer since blocks might not be contiguous. Random accesses are horrible and  can  involve multiple sequential
Searches which will suck time and energy better spent elsewhere.  Lastly, linked list allocation can be unreliable, since missing a single pointer can lead to loss of the remaining file. 

Segment Based Allocation
Segment based allocation uses a segment table to allocate to  multiple regions of contiguous blocks. Segment based allocation provides the flexibility required to  separate into a number of contiguous disk  regions,  while  also  permitting  contiguous  allocation  to  reduce  disk  seek
time.    However,  as  the  disk  becomes  increasingly  fragmented,   it will need a bigger and bigger table to locate piecewise contiguous blocks.    In an extreme  case,  segment based  allocation  can  potentially  need  one  table  entry per disk block.  In addition
, under this scheme, random accesses are not as fast as the contiguous allocation. Because   the file system needs to locate the pair of  begin  and  end blocks that contain the target byte before making the disk accesses.


Indexed Allocation
Indexed  allocation allows the use of  an index to track the file block locations.  A  user  declares the maximum file size, and the file system allocates a file header with an array of pointers big enough to point to all file blocks.


Although indexed allocation provides fast disk location lookups for random accesses  the file

blocks  might  be  scattered  all  over  the  disk making it potentially difficult to find.    A  file  system  needs  to  provide  additional mechanisms to ensure that disk blocks are grouped together for good and speedy performance.  Also,  as  the  file  increases  in  size, its system  needs  to reallocate the index array and copy old entries. So the index can ideally grow incrementally.


Multilevel Indexed Allocation:
Linux uses multilevel indexed allocation, so certain index entries point to the index blocks as opposed  to  data  blocks.    The  file  header,  or  the i node data  structure   holds  around 15  index pointers. The  first 12  pointers  point  to the  data  blocks.  The 13th pointer  points  to  one single indirect block , which contains 1,024 additional pointers pointing to data blocks.  The 14th pointer in  the  file  header  points  to  a double  indirect  block,  which  contains  1,024  pointers  to a bunch of single indirect blocks.  The 15th pointer points to a triple indirect block, which contains 1,024 pointers to the double indirect blocks.  


 This skewed multilevel index tree is tailored  for both small and large files.  Small files can be accessed through the first 12 pointers, while large files can grow with incremental allocations of  the index blocks.  However accessing the  data block under the triple indirect block involves multiple disk accesses: one disk access for the triple indirect block. 
 Another for the  double indirect block, and yet another for the single indirect block before accessing the actual data block. 

 Also, the number of pointers provided by this data structures limited  the largest file size.  Lastly, the boundaries between the last four pointers are somewhat arbitrary.  With the given block number, it is not immediately obvious as to which of the 15 pointers to follow.




Inverted Allocation

If  we  use  a  disk  as  a  device  for  backups   the  storage  capacity  will  be  the primary concern, and the speed of the disk may not matter but it must be faster than the tape. 
Since backup storage keeps track of all modifications, a small modification to a large file
results  in the storage of  the  entire modified  file. Inverted  allocation allocates  the disk  block  by hashing the file block content to the disk block location.  By doing so the different file blocks 
of the same content  can  share the same disk block for storage purposes.  For
example, if one block is modified in the  N block file, then the storage requirement for both files
is N + 1 blocks. 





Disk Structure:

Disks  provide  the  bulk  of  secondary  storage.  They
come  in  multi platter  disk  packs  and have all the following characteristics:

•
Each platter is divided into several  different tracks.

•
Each track has several different sectors.
.

•
Each platter has two different surfaces.
.
A Cylinder is  the  set  of  tracks which  are  at  the  same  track  position  on  all  disk

surfaces.

•
The  number  of  sector, bytes, tracks,platter and track  vary  among  disk types.

•
To  access  a  sector,  one  must  know  the  following components drive number,  track/cylinder,  surface  and

sector.

•
A sector is the smallest data section that can be read and written on a block device.



Free sequence list

Usually free disk blocks are in contiguous chunks. Thus we need only keep a 
pointer to the  first  free  block  of  each  sequence  and  record  the  number  of  blocks  in  the  sequence.

This  can  be  used  for  any  allocation  method  (but  is  particularly  efficient  for  contiguous

allocation), and it is easily stored. However, it does lead to disk fragmentation.


DISK SPACE ALLOCATION METHODS


Contiguous Allocation

Each file is allocated to contiguous disk blocks. For each file, the device directory contains

the location of the  starting block and the number of blocks within the file.


Access  is pretty easy.  Sequential  and  direct  access  is  possible. This  allocation method  also reduces  head  movement  during  file  access with most of the  blocks on  the  same cylinder.


Finding  space  for  a  new  file can be awkward and stressful.  The  free  space  list  must  be  searched  for  a  big enough chunk of contiguous space to find space for the file.
When a   disk  is  made  up  of  allocated  and  free  sequences  of  blocks the  sequences  are  called holes.


First Fit means the  Allocation of the  first hole that fits the file.

Best Fit means the Allocation of the smallest hole that fits the file.

Worst fit means the Allocation of the biggest hole available.

 The  dynamic  allocation  problem  is  that  of  finding  the  most suitable hole for a file of size n blocks.  In this situation the 3 best algorithms are First fit,Best Fit and Worst fit. Which are prone  to external fragmentation. The Two possible solutions are  either compaction or abandon contiguous allocation.

Contiguous allocation has other problems and constraints. The size of the file must be known. But it usually isn’t known until the problem presents itself. Thus as the file grows it may have to be moved  around the disk in order to  find  a file big enough  space for its new size.

Trying to anticipate  growth  by  over allocating  space  will lead  to  internal  fragmentation as the  extra space may never be used.


Chained (Linked) Allocation:


The  above  problems  are  due  to  the  need  to  allocate  contiguous  space.
Linked  allocation is designed to avoids  those  problems.  This  is achieved by allowing disk  space  to  be allocated randomly. A file’s space will be a linked list of blocks. Pointers are maintained to in the  first and last blocks.  Each  block  points  to  their successor.  So that each file and the device directory can contain the location of its starting blocks.


Access is quite problematic. Only sequential access is possible. The way to access a random block, the  access  must  begin  at  the  start  and  follow all the pointers.  Finding  space  for  files  is  usually pretty easy.  Blocks  are  usually chosen  from  the  free  space  list  and linked to the previous blocks. But there is no external fragmentation.


Another  minor  problem  is  that the  space is used  up  by pointers. Or more accurately the  pointers scattered all over the disk which increases the possibility of losing a part of a file due to a corrupt pointer.  One partial  solution to this is to use  doubly  linked lists.  Another solution involves storing the file name  and relative  block  node in  each  block.  But this requires extra space overhead.


Indexed Allocation: 
Linked allocation can solve the external fragmentation and file growth problems of contiguous allocation. However, it also introduces some problems of its own. For example no direct access and scattered block pointers. But Indexed allocation does address both of these problems without the problems of contiguous allocation which is a plus. 

All  of  a  file’s  block  pointers  are  gathered  together  into  its index  block  which is an  array  of  pointers to blocks that are stored on the disk. The ith entry is supposed be  the pointer to the file’s ith block. 

Access  is  pretty  easy.  Both  sequential  and  direct  access  possible.  There is no  external  fragmentation with the Allocation being relatively easy to manage. The Files also allocaticates extra blocks from the free space list as they grow in size.
Disk Scheduling:

For a single disk there will be multiple I/O requests. If requests are selected randomly, the user will get the worst possible performance. As a consequence several algorithms exist to schedule the servicing of disk I/O requests.
First Come First Serve (FCFS) is when the  I/O requests are served in the order in which they reach.
Shortest Seek Time First  (SSTF) is  the process of selecting the request  with the minimum  seek time from the current head position.
SCAN  Scheduling is when the disk  arm  starts  at  one  end  of  the  disk,  and  moves  toward  the other  end initiating servicing  requests  until  it  gets  to  the  other  end of the disk where  the  head movement is reversed and servicing continues.
Circular Scan  (C Scan) is when the  head  moves from one  end  of  the  disk  to  the  other servicing  requests  as  it  goes. But  when  it  reaches  the  other  end it immediately returns to the beginning of the disk, without servicing any requests on the return trip.
Circular Look  (C- Look) Arm only goes  as  far as  the last  request  in each direction before reversing the direction immediately, without first going all the way to the end of the disk.


Chapter notes please th

Parallel ATA also known as Parallel Advanced Technology Attachment is the standard for connecting hard drives to computer systems. PATA is based on computer signal technology which is a method of conveying multiple binary bits simultaneously. 

Serial Advanced Technology  Attachment is a command and transport protocol that defines how data is transferred between the motherboard and hard drive. One of these hard drive drive types called a solid state drive is when the hard drive uses flash memory instead of the classic mechanical disk. SSD is also much faster and has higher storage capacity than its counterpart.  Another variation of SSD called  SSHD (Solid State Hybrid drive) uses a particular type of flash memory called NAND to hold  important data on a hard disk drive device.   RAID is a way of storing the same data in different places on multiple SSD or hard disk drives to protect the data from potential drives failures. 


Explanation as to why the notes from the textbook are extremely limited/ crappy only pertains to information supposed to be provided by the o'riley textbook:

I currently am having issues with accessing different pages of the learning oreilly learning textbook. It is currently only letting me access one page and even though I selected another page it refuses to go to that particular page or any page minus the first one at all.  Further the only page I have access provides barely anything more than a couple of cliff notes. So I had to google most of the information to at least having something written in vain attempt to provide a good supplement and not just cheap out and do anything at all for the textbook notes. I would have if the technical issue never happened.  Sorry for any confusion or inconvenience and please take this into consideration when grading the assignment.  I am also currently writing this after a time in which the IT department on Monday september 26th is closed so I am unable to get help on this particular issue. In case you don’t believe me, The google docs file that I sumbittted along with the link contains an image of the issue to illustrate the problem I'm currently experiencing. 

 
 
[mrees66/tech-journal-MRees](https://www.github.com)
![Screenshot (98)](https://user-images.githubusercontent.com/112408713/192362912-6e829112-2730-4f44-8455-e027124fdd25.png)
