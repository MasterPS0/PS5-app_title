PS5_HOST ?= ps5
PS5_PORT ?= 9021

ifdef PS5_PAYLOAD_SDK
  include $(PS5_PAYLOAD_SDK)/toolchain/prospero.mk
else
  $(error PS5_PAYLOAD_SDK is undefined)
endif

CC      := $(PS5_PAYLOAD_SDK)/bin/prospero-clang
CFLAGS  := -Wall -Werror -g \
           -I$(PS5_PAYLOAD_SDK)/includes
LDFLAGS :=

ELF     := image_processor.elf
SRC     := main.c

all: $(ELF)

$(ELF): $(SRC)
	$(CC) $(CFLAGS) -o $@ $<

clean:
	rm -f $(ELF)

test: $(ELF)
	$(PS5_DEPLOY) -h $(PS5_HOST) -p $(PS5_PORT) $^

debug: $(ELF)
	gdb \
		-ex "target extended-remote $(PS5_HOST):2159" \
		-ex "file $(ELF)" \
		-ex "remote put $(ELF) /data/$(ELF)" \
		-ex "set remote exec-file /data/$(ELF)" \
		-ex "start"
