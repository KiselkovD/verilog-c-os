# RISC-V Verilog C OS

This project provides a complete infrastructure for running C programs and operating systems on a 32-bit RISC-V architecture with both ISA simulation and Verilog RTL simulation support.

## Project Structure

The project is organized into two separate versions, each with its own Docker configuration and dependencies:

```
verilog-c-os/
├── README.md                   # This file - main project documentation
├── LICENSE                     # Project license
├── .gitignore                  # Git ignore patterns
│
├── xv6_version/                # xv6-like OS version
│   ├── README.md               # Documentation for xv6 version
│   ├── docker-compose.yml      # Docker Compose config for xv6
│   ├── docker_image/           # Docker image configuration
│   │   ├── Dockerfile          # Docker image with all dependencies
│   │   └── entrypoint.sh       # Container entrypoint script
│   ├── xv6/                    # xv6-like kernel source
│   │   ├── xv6_kernel.c        # Simplified xv6-like OS kernel
│   │   ├── startup.s           # Assembly startup code
│   │   ├── link_script.ld      # Linker script
│   │   ├── Makefile            # Build system
│   │   ├── run_xv6.sh          # Run script
│   │   └── build/              # Build output directory
│   ├── core/riscv/             # RISC-V processor core (Verilog)
│   ├── isa_sim/                # ISA simulator (C++ model)
│   ├── top_tcm_axi/            # Top-level testbench with TCM
│   └── doc/                    # Documentation images
│
└── run_c_version/              # Standalone C programs version
    ├── README.md               # Documentation for run_c version
    ├── docker-compose.yml      # Docker Compose config for run_c
    ├── docker_image/           # Docker image configuration
    │   ├── Dockerfile          # Docker image with all dependencies
    │   └── entrypoint.sh       # Container entrypoint script
    ├── run_c/                  # C program infrastructure
    │   ├── run.c               # Example C program
    │   ├── lib/                # Support library
    │   │   ├── startup.s       # Assembly startup code
    │   │   ├── link_script.ld  # Linker script
    │   │   └── utils.c         # Utility functions
    │   ├── Makefile            # Build system
    │   ├── run_c_program.sh    # Run script for any C program
    │   └── build/              # Build output directory
    ├── core/riscv/             # RISC-V processor core (Verilog)
    ├── isa_sim/                # ISA simulator (C++ model)
    ├── top_tcm_axi/            # Top-level testbench with TCM
    └── doc/                    # Documentation images
```

## Quick Start

### Running xv6-like OS

```bash
cd xv6_version
docker compose up xv6
```

### Running C Programs

```bash
cd run_c_version
docker compose up run_c
```

To run a custom C program:
```bash
cd run_c_version
docker compose run --rm run_c bash -c "cd /workspace/run_c && ./run_c_program.sh your_program.c"
```

## Versions

### xv6_version
This version implements a simplified xv6-like operating system kernel with:
- Process table management
- Memory allocation system
- Basic OS functionality
- Compatible with 32-bit RISC-V simulation

See [xv6_version/README.md](xv6_version/README.md) for detailed documentation.

### run_c_version
This version provides infrastructure for running standalone C programs with:
- Simple compilation to RISC-V ELF
- Support for custom C programs
- Reusable library with startup code and linker scripts
- Simulation control primitives

See [run_c_version/README.md](run_c_version/README.md) for detailed documentation.

## Simulation Modes

Both versions support two simulation modes:

1. **ISA Simulator** - Fast C++ model that executes instructions quickly
2. **Verilog RTL Simulator** - Cycle-accurate hardware simulation with VCD waveforms

## Dependencies

All dependencies are included in the Docker images:
- RISC-V GCC toolchain (riscv64-unknown-elf-gcc)
- SystemC 2.3.3
- Verilator
- Build tools (make, g++, libelf-dev, binutils-dev)

## Manual Installation (Without Docker)

If you prefer to run without Docker, install the following dependencies:

```bash
# Install RISC-V toolchain
sudo apt-get install gcc-riscv64-unknown-elf

# Install SystemC
wget https://github.com/accellera-official/systemc/releases/download/2.3.3/systemc-2.3.3.tar.gz
tar -xzf systemc-2.3.3.tar.gz
cd systemc-2.3.3
mkdir build && cd build
../configure --prefix=/usr/local/systemc-2.3.4
make && sudo make install

# Install Verilator
sudo apt-get install verilator

# Install build dependencies
sudo apt-get install build-essential libelf-dev binutils-dev
```

## Output Files

After running simulations, the `build/` directory in each version will contain:
- `.elf` - RISC-V executable
- `.disasm` - Disassembly of the program/kernel
- `_isa.log` - ISA simulator log
- `_verilog.log` - Verilog simulator log
- `_sysc_wave.vcd` - SystemC waveform
- `_verilator.vcd` - Verilator waveform with internal processor signals

## License

See [LICENSE](LICENSE) for licensing information.

---
ru
---

# ОС Verilog C для RISC-V

Этот проект предоставляет полную инфраструктуру для запуска программ и операционных систем на языке C на 32-битной архитектуре RISC-V с поддержкой как моделирования набора инструкций (ISA), так и моделирования RTL на языке Verilog.

## Структура проекта

Проект организован в две отдельные версии, каждая со своей конфигурацией Docker и зависимостями:

```
verilog-c-os/
├── README.md               # Этот файл - основная документация проекта
├── LICENSE                 # Лицензия проекта
├── .gitignore              # Шаблоны игнорирования Git
│
├── xv6_version/            # Версия ОС, подобная xv6
│ ├── README.md             # Документация для версии xv6
│ ├── docker-compose.yml    # Конфигурация Docker Compose для xv6
│ ├── docker_image/         # Конфигурация образа Docker
│ │ ├── Dockerfile          # Образ Docker со всеми зависимостями
│ │ └── entrypoint.sh       # Скрипт точки входа контейнера
│ ├── xv6/                  # Исходный код ядра, подобного xv6
│ │ ├── xv6_kernel.c        # Упрощенное ядро ​​ОС, подобное xv6
│ │ ├── startup.s           # Код запуска на ассемблере
│ │ ├── link_script.ld      # Скрипт компоновщика
│ │ ├── Makefile            # Система сборки
│ │ ├── run_xv6.sh          # Скрипт запуска
│ │ └── build/              # Каталог вывода сборки
│ ├── core/riscv/           # Ядро процессора RISC-V (Verilog)
│ ├── isa_sim/              # Симулятор архитектуры набора команд (модель C++)
│ ├── top_tcm_axi/          # Тестовый стенд верхнего уровня с TCM
│ └── doc/                  # Образы документации
│
└── run_c_version/          # Версия автономных программ на C
    ├── README.md           # Документация для версии run_c
    ├── docker-compose.yml  # Конфигурация Docker Compose для run_c
    ├── docker_image/       # Конфигурация образа Docker
    │ ├── Dockerfile        # Образ Docker со всеми зависимостями
    │ └── entrypoint.sh     # Скрипт точки входа контейнера
    ├── run_c/              # Инфраструктура программы на C
    │ ├── run.c             # Пример программы на C
    │ ├── lib/              # Вспомогательная библиотека
    │ │ ├── startup.s       # Код запуска на ассемблере
    │ │ ├── link_script.ld  # Скрипт компоновщика
    │ │ └── utils.c         # Вспомогательные функции
    │ ├── Makefile          # Система сборки
    │ ├── run_c_program.sh  # Скрипт запуска для любой программы на C
    │ └── build/            # Каталог вывода сборки
    ├── core/riscv/         # Ядро процессора RISC-V (Verilog)
    ├── isa_sim/            # Симулятор архитектуры набора команд (модель C++)
    ├── top_tcm_axi/        # Тестовый стенд верхнего уровня с TCM
    └── doc/                # Образы документации
```

## Быстрый старт

### Запуск ОС, подобной xv6

```bash
cd xv6_version
docker compose up xv6
```

### Запуск программ на C

```bash
cd run_c_version
docker compose up run_c
```

Чтобы запустить пользовательскую программу на C:
```bash
cd run_c_version
docker compose run --rm run_c bash -c "cd /workspace/run_c && ./run_c_program.sh your_program.c"
```

## Версии

### xv6_version
Эта версия реализует упрощенное ядро ​​операционной системы, подобное xv6, со следующими особенностями:
- Управление таблицей процессов
- Система распределения памяти
- Базовая функциональность ОС
- Совместимость с 32-битным моделированием RISC-V

Подробную документацию см. в файле [xv6_version/README.md](xv6_version/README.md).

### run_c_version
Эта версия предоставляет инфраструктуру для запуска автономных программ на языке C со следующими особенностями:
- Простая компиляция в формат RISC-V ELF
- Поддержка пользовательских программ на языке C
- Многократно используемая библиотека с кодом запуска и скриптами компоновщика
- Примитивы управления моделированием

Подробную документацию см. в файле [run_c_version/README.md](run_c_version/README.md).

## Режимы моделирования

Обе версии поддерживают два режима моделирования:

1. **Симулятор ISA** — быстрая модель C++, которая быстро выполняет инструкции
2. **Симулятор Verilog RTL** — моделирование аппаратного обеспечения с точностью до такта и использованием VCD-сигналов

## Зависимости

Все зависимости включены в образы Docker:
- Инструментарий RISC-V GCC (riscv64-unknown-elf-gcc)
- SystemC 2.3.3
- Verilator
- Инструменты сборки (make, g++, libelf-dev, binutils-dev)

## Ручная установка (без Docker)

Если вы предпочитаете работать без Docker, установите следующие зависимости:

```bash
# Установка инструментария RISC-V
sudo apt-get install gcc-riscv64-unknown-elf

# Установка SystemC
wget https://github.com/accellera-official/systemc/releases/download/2.3.3/systemc-2.3.3.tar.gz
tar -xzf systemc-2.3.3.tar.gz
cd systemc-2.3.3
mkdir build && cd build
../configure --prefix=/usr/local/systemc-2.3.4
make && sudo make install

# Установка Verilator
sudo apt-get install verilator

# Установка зависимостей сборки
sudo apt-get install build-essential libelf-dev binutils-dev
```

## Выходные файлы

После запуска симуляций каталог `build/` в каждой версии будет содержать:
- `.elf` - исполняемый файл RISC-V
- `.disasm` - дизассемблированный код программы/ядра
- `_isa.log` - Журнал симулятора ISA
- `_verilog.log` - Журнал симулятора Verilog
- `_sysc_wave.vcd` - Сигнал SystemC
- `_verilator.vcd` - Сигнал Verilator с внутренним сигналом процессора