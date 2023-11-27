ifeq ($(OS),Windows_NT)
	CFLAGS = -Iinclude -Llib -Wall
	LDFLAGS = -lmingw32 -lSDL2main -lSDL2 -lSDL2_mixer -lSDL2_ttf -lm -g
else
	CFLAGS = -Iinclude -Llib
	LDFLAGS = -lSDL2main -lSDL2 -lm
endif

compilar : src/visualizacion.c
	@gcc $(CFLAGS) src/visualizacion.c -o bin/visualizacion.exe $(LDFLAGS)
.PHONY :  run debug

run : bin/visualizacion.exe
	@./bin/visualizacion.exe
debug : bin/visualizacion.exe
	@gdb ./bin/visualizacion.exe
