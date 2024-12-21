# first-fit-algorithm
EEX5564 mini project 2023/2024
class MemoryBlock:
    def __init__(self, size):
        self.size = size
        self.is_free = True

    def allocate(self, size):
        if self.is_free and self.size >= size:
            self.is_free = False
            allocated_size = size
            self.size -= size
            return allocated_size
        return 0

    def free(self, size):
        self.is_free = True
        self.size += size

class MemoryAllocator:
    def __init__(self, memory_sizes):
        self.blocks = [MemoryBlock(size) for size in memory_sizes]

    def allocate_memory(self, process_id, size):
        for i, block in enumerate(self.blocks):
            if block.is_free and block.size >= size:
                block.allocate(size)
                print(f"Process {process_id} allocated {size} units in Block {i + 1}.")
                return True

        print(f"Process {process_id} could not allocate {size} units: Insufficient memory.")
        return False

    def deallocate_memory(self, process_id, block_index, size):
        if 0 <= block_index < len(self.blocks):
            block = self.blocks[block_index]
            if not block.is_free:
                block.free(size)
                print(f"Process {process_id} deallocated {size} units from Block {block_index + 1}.")
                return True
        print(f"Process {process_id} could not deallocate {size} units from Block {block_index + 1}: Invalid operation.")
        return False

    def display_memory(self):
        print("Memory Blocks:")
        for i, block in enumerate(self.blocks):
            status = "Free" if block.is_free else "Allocated"
            print(f"  Block {i + 1}: {block.size} units ({status})")

if __name__ == "__main__":
    memory_sizes = [100, 50, 30, 120, 60]
    allocator = MemoryAllocator(memory_sizes)

    print("Initial Memory State:")
    allocator.display_memory()

    # Simulate allocation requests
    allocator.allocate_memory(1, 20)
    allocator.allocate_memory(2, 50)
    allocator.allocate_memory(3, 15)
    allocator.allocate_memory(4, 60)

    print("\nMemory State After Allocations:")
    allocator.display_memory()

    # Simulate deallocation requests
    allocator.deallocate_memory(2, 1, 50)

    print("\nMemory State After Deallocation:")
    allocator.display_memory()
