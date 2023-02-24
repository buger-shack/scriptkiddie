# CUPS

## Brief

> CUPS (formerly an acronym for Common UNIX Printing System) is a **modular printing system** for **Unix-like computer** operating systems which **allows a computer to act as a print server**. A computer running CUPS is a host that can accept print jobs from client computers, process them, and send them to the appropriate printer. CUPS consists of a **print spooler** and **scheduler**, a filter system that converts the print data to a format that the printer will understand, and a backend system that sends this data to the print device. CUPS uses the **Internet Printing Protocol (IPP)** as the basis for managing print jobs and queues.

## Scanning

```bash
# run all cups-related nse scripts
nmap -sV -p631 --script=cups* $target

# lists printers managed by the CUPS printing service
nmap -sV -p631 --script=cups-info $target

# lists currently queed print jobs of the remote CUPS service grouped by printer
nmap -sV -p631 --script=cups-queue-info $target
```
