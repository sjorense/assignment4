# Assignment 4 Calculator

A command-line calculator implemented in Python. The application provides an
interactive REPL for arithmetic operations and uses an object-oriented design to
separate user interaction, calculation objects, and low-level arithmetic
operations.

## Features

- Interactive calculator prompt.
- Supports `add`, `subtract`, `multiply`, `divide`, and `power`.
- Session history for completed calculations.
- Clear error messages for invalid input, unsupported operations, and division
  by zero.
- Calculation factory pattern for registering and creating operation-specific
  calculation classes.
- Pytest test suite with coverage reporting.

## Project Structure

```text
assignment4/
├── app/
│   ├── calculator/      # Interactive REPL and display helpers
│   ├── calculation/     # Calculation classes and factory registration
│   └── operation/       # Stateless arithmetic operations
├── tests/               # Pytest test suite
├── main.py              # Application entry point
├── pytest.ini           # Pytest and coverage configuration
├── requirements.txt     # Development and test dependencies
└── README.md            # Project documentation
```

Generated directories such as `venv/`, `htmlcov/`, and `__pycache__/` are not
required source files.

## Requirements

- Python 3.12 or compatible Python 3 version.
- `pip` for installing dependencies.

## Setup

From the project root:

```bash
cd /Users/stevenorense/projects/assignment4
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

If you already have the included virtual environment configured, activate it
with:

```bash
source venv/bin/activate
```

## Usage

Start the calculator:

```bash
python main.py
```

At the `>>` prompt, enter an operation followed by two numbers:

```text
add 10 5
subtract 15.5 3.2
multiply 7 8
divide 20 4
power 2 3
```

Special commands:

- `help` displays usage instructions.
- `history` shows calculations completed during the current session.
- `exit` closes the calculator.

Example session:

```text
Welcome to the Professional Calculator REPL!
Type 'help' for instructions or 'exit' to quit.

>> add 10 5
Result: AddCalculation: 10.0 Add 5.0 = 15.0

>> history
Calculation History:
1. AddCalculation: 10.0 Add 5.0 = 15.0

>> exit
Exiting calculator. Goodbye!
```

## Running Tests

Run the test suite:

```bash
pytest
```

The project is configured to collect tests from `tests/` and report coverage for
the `app/` package. A terminal coverage summary is printed, and an HTML report is
generated in `htmlcov/`.

To view missing coverage in the terminal only:

```bash
pytest --cov=app --cov-report=term-missing
```

## Design Notes

The project is organized around three layers:

- `Operation` contains stateless static methods for arithmetic.
- `Calculation` subclasses wrap operands and delegate arithmetic to
  `Operation`.
- `CalculationFactory` maps user-facing operation names to calculation classes.

New operations can be added by creating a `Calculation` subclass, implementing
`execute`, and registering it with
`@CalculationFactory.register_calculation("<operation-name>")`.

For example:

```python
@CalculationFactory.register_calculation("modulus")
class ModulusCalculation(Calculation):
    """Calculate the remainder after dividing one operand by another."""

    def execute(self) -> float:
        return self.a % self.b
```

If the operation should be available from the REPL, also update the help text and
add tests for the operation and CLI behavior.
