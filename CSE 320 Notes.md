# Memory

### Memory Hierarchies
- Def: an approach for organizing memory and storage systems
- CPU growth is faster; RAM growth is slower; Gap is widening
- ![[Pasted image 20260903123731.png]]

### Locality:
- Want to use things that are near other things
- **Temporal**: (time) recently referenced items that are likely to be referenced again in the future
	- reference variable *sum* each iteration
	- Cycle through loop repeatedly
- **Spatial**: (space) items with nearby addresses tend to be referenced close together in time
	- reference array elements in sucession
	- reference instructions in sequence

### Disk Drive:
- Inside: Platters (the disk), spindle (the thing spinning the disk), arm (reading the disk), actuator (assist the arm), SCSI connector, Electronics
- Each **platter** contain 2 surfaces
- Each **surface** contains concentric rings called tracks
- Each **track** consists of sectors separated by gaps
- Capacity = max number of bits that can be stored =  (# bytes/sector) x (avg. # sectors/track) x (#tracks/surface) x (# surfaces/platter) x (# platters/disk) = N bytes/disk
- Why its slow: It can only spin so fast. You are waiting for seek time or transfer time
- **Disk Access Time**:
	- Taccess = avg time to access some target sector = Tavg seek + Tavg rotation + Tavg transfer
	- Tavg seek = Typical Tavg seek is 3—9 ms
	- Tavg rotation = 1/2 x 1/RPMs x 60 sec/1 min
	- Tavg transfer = 1/RPM x 1/(avg # sectors/track) x 60 secs/1 min

#### Logical Disk Blocks:
- Mapping between logical blocks and actual (physical) sectors
- Allows controller to set aside spare cylinders for each zone.
	- Accounts for the difference in “formatted capacity” and “maximum capacity”.

#### I/O Bus:
- **DMAs**:
	- Move large amounts of data from Memory to Disk (or vice versa)
	- CPU tells what the DMA to do, and then the DMA does the job
- **Interrupts**:
	- The only controller that has a pin into the CPU
	- Sends the message to CPU
	- Could make the CPU check the "mailbox" periodically (polling)
- Analogy: CPU = Orchestrator, Interrupts = Reviewer and Reporter, DMA = subagent

### Solid State Drives (SSDs):
- Blocks contain Pages
- Can only write to a page once per block per page
	- Any changes requires the need to erase the block and rewrite all the pages
- A block wears out after about 100,000 repeated writes.
- Sequential read/write is faster than Random read/write (Referring to locality? As in spatial locality being faster?)

### SSD Tradeoffs vs Rotating Disks
- Advantages
	- No moving parts -> means faster
- Disadvantages
	- Have potential to wear out
	- More expensive

### Random-Access Memory (RAM):
- Dynamic Ram -> DDR
- SRAM (static RAM)
	- Cache
- DRAM (Dynamic Ram)
	- Main memories, frame buffers
- DRAM and SRAM are volatile (meaning subject to change) RAM
	- Lose info if powered off

### RAID:
- Raid 0: store everything once
- Raid 1: Most naive backup. Save everything twice to 2 different hard drives
- Raid 2-6 more efficient

### Caches:
- A smaller, faster storage device that acts as a staging area for a subset of the data in a larger, slower device.
- Big Idea: The memory hierarchy creates a large pool of storage that costs as much as the cheap storage near the bottom, but that serves data to programs at the rate of the fast storage near the top.
- **Exploiting TEMPORAL AND SPATIAL LOCALITY!!!**

# Caches

### Caches
- See definition above
- Caches - Smaller, faster, more expensive memory caches a subset of the blocks
- Memory - larger slower, cheaper memory viewed as partitioned into "blocks"

### Cache Memories
- Small, fast SRAM-based memories managed automatically in hardware
- CPU looks first for data in cache

### Cache Architectures:
- Direct Mapped
	- Every address in RAM has 1 cached block that it can use
	- Lines = 1
- Fully Associative
	- "No assigned seats"
	- Any RAM address can be stored in any cache block
	- Sets = 1
- Set-Associative (Basically the compromise between the two above)
	- Every address in RAM has 1 cache set that it can use
	- Middle ground idea

### General Cache Organization (S. E. B.)
- Cache Size: C = S * E * B data bytes
- Blocks = S * E
- ![[Pasted image 20260908155304.png]]
- ![[Pasted image 20260908162227.png|210]]
- Block offset and set index = where the word is
- Tag = checks if the contents are correct
- E-way Set associative cache
	- E = the number of lines
- Block = even | odd

### Writes
- Copies of Data: L1, L2, L3, Main Memory, Disk
- Write-hit:
	- Write-through (write immediately to memory)
	- Write-back (defer write to memory until replacement of line)
		- Need a dirty bit (line different from memory or not)
		- Higher performing than write through
		- Three events to trigger
			1. Explicitly say to do a write back
			2. operating system has switched process for you and forced a write back
			3. Eviction (program uses all the cache so blocks gotta be evicted, thus initiating write backs)
- Write-miss:
	- Write-allocate (load into cache, update line in cache)
		- Good if more writes to the location follow
	- No-write-allocate (writes straight to memory, does not load into cache)
- Typical:
	- Write-through + No-write-allocate
	- **Write-back + Write-allocate**

### Cache Eviction
- Replacement strategy - removing cache data to make space for new data
- No decision for direct mapped cache
- Need to decide for set-assoicative cache
- Different policies that exist:
	- Optimal/Clairvoyant - kinda acts like a benchmark but not a good strategy for IRL
		- Not a good strat because it's literally impossible to implement in the real world; it requires knowing of the future 
	- Random
	- First-in First-out (FIFO)
		- Timestamp the block loading, pick the oldest
		- Not a very good strat (but still solid) because oldest block might also be your most used block (might actually be the program itself)
	- Least Recently Used (LRU)
		- Timestamp the block access, pick the oldest
		- Better caching than FIFO but you are paying a heavy costs on cache hits
			- Different than FIFO since you are marking and updating the timestamp
			- Expensive since it's not just a read, but also requires a write to reorder the data
	- Least Frequently Used (LFU)
		- Count the number of accesses to a line
		- Similar to LRU but a little bit less precise

### Intel Core i7 Cache Hierarchy
- L1 i-cache & d-cache:
	- 32 KB,  8-way, 
	- Access: 4 cycles
- L2 unified cache:
	- 256 KB, 8-way, 
	- Access: 10 cycles
- L3 unified cache:
	- 8 MB, 16-way,
	- Access: 40-75 cycles
- Block size: 
	- Always 64 bytes
- ![[Pasted image 20260910160508.png]]

### Cache Performance Metrics
- Miss Rate
	- **Why track Miss Rate than Hit Rate?**
		- Hit Rate usually has higher numbers and Miss Rates are lower. I guess Miss Rates are more attractive
		- Consider:
			- Cache hit time of 1 cycle
			- miss penalty of 100 cycles
		- Average access time:
			- 97% hits: 1 cycle + 0.03 * 100 cycles = 4 cycles
			- 99% hits: 1 cycle + 0.01 * 100 cycles = 2 cycles
		- Miss rate reveals the real-world impact. Miss rate from 3% to 1% is 3x reduction. A much more attractive number.
	- Typical numbers:
		- 3-10% for L1
		- can be quite small (e.g. < 1%) for L2, depending on size, etc.
- Hit Time
	- The amount of time to deliver a line in the cache to the processor
		- includes time to determine whether the line is in the cache
	- Typical numbers:
		- 4 clock cycle for L1
		- 10 clock cycles for L2
- Miss Penalty
	- Additional time required because of a miss
		- typically 50-200 cycles for main memory (Trend: increasing!)

### Writing Cache Friendly Code
- Make common case go fast
	- Focus on inner loops of the core functions
- Minimize the misses in the inner loops
	- Repeated references to variables are good (temporal locality)
	- Stride-1 ("means your program accesses contiguous memory locations one immediately after the other") reference patterns are good (spatial locality)
- **Key idea: Our qualitative notion of locality is quantified through our understanding of cache memories**

### The Memory Mountain:
- Taller is faster, shorter is slower to access.
- Most cache hits occur on the ridge across the top of the mountain
![[Pasted image 20260910161842.png]]

### Matrix Multiplication
- See slides for implementations and explanations
- Traversing row-wise is more efficient than traversing column-wise
	- Row-wise is more cache hits (size($a_{ij}$) / Block)
		- So for doubles it's "miss hit hit hit" for every 4 elements
	- Column-wise is all cache misses




#### MT 1

- All MCQs
- **Guaranteed One question for each of: Disk allocation + Disk calculation question**
- Some similar questions to cache simulation