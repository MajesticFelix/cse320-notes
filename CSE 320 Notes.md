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

