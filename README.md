# gba-huff

C++14 Huffman compressor for the GBA, based on [gba-hpp](https://github.com/felixjones/gba-hpp).


## Usage

There's two functions, one for 4bit mode encoding and another one for 8bit mode encoding:

```cpp
#include "gba_hpp.h"

int main()
{
    const std::uint8_t* input_data = ...;
    std::size_t input_data_size = ...;
    gba_huff::result_type huffman_4 = gba_huff::compress_4(input_data, input_data_size);
    gba_huff::result_type huffman_8 = gba_huff::compress_8(input_data, input_data_size);
    const gba_huff::result_type* best_huffman = huffman_4.data.size() < huffman_8.data.size() ?
                &huffman_4 : &huffman_8;
				
    std::size_t output_header_size = sizeof(gba_huff::bios_uncomp_header);
    std::size_t output_data_size = best_huffman->data.size();
    std::size_t output_total_size = header_size + data_size;
    auto output_data = static_cast<std::uint8_t*>(std::malloc(output_total_size));
    std::memcpy(output_data, &best_huffman->header, output_header_size);
    std::memcpy(output_data + output_header_size, best_huffman->data.data(), output_data_size);
	std::free(output_data);
	
    return 0;
}
```


## License

gba-huff is licensed under the zlib license, see the [LICENSE.md](LICENSE.md) file for details.
