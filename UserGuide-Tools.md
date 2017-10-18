[[Home]] > [[User Guide|UserGuide]] > Tools

# Tools 

Besides our developer APIs we also produce a suite of command line and GUI tools for working with RDF and SPARQL, you can find links to their documentation in this section of the user guide.

## Download and Pre-requisites

You can download the dotNetRDF toolkit from our [GitHub Repository](https://github.com/dotnetrdf/dotNetRDF.Toolkit/releases). The tools are released in two packages:

* `dotNetRDFToolkit-Installer-*.zip` is a zip file containing a setup.exe and MSI installer package for the tools. Download and unzip and then run the setup.exe to install the tools. This is also the smaller of the two packages as shared files are included in the installer only once.

* `dotNetRDFToolkit-noInstaller-*.zip` is a zip file containing the binaries. You can simply download and unzip the files to what ever location is most convenient for you.

NOTE: Using the Toolkit requires .Net 4.0 Framework Full to be installed on your machine.

## Available Tools 

| Tool | Description |
| --- | --- |
| [rdfConvert](UserGuide-Tools-rdfConvert) | Utility for converting between different RDF formats |
| [rdfEditor](UserGuide-Tools-rdfEditor) | Notepad replacement for editing of RDF files |
| [rdfOptStats](UserGuide-Tools-rdfOptStats) | Utility for generating stats for use with our in-memory stats based optimizer |
| [rdfQuery](UserGuide-Tools-rdfQuery) | Utility for making SPARQL queries at the command line |
| [rdfServer](UserGuide-Tools-rdfServer) | Utility for running a simple SPARQL server |
| [rdfServerGui](UserGuide-Tools-rdfServerGui) | Simple GUI for managing rdfServer instances |
| [rdfWebDeploy](UserGuide-Tools-rdfWebDeploy) | Utility for helping with deployment of our [ASP.Net Integration](UserGuide-ASPNET-Integration) features |
| [soh](UserGuide-Tools-soh) | Utility for accessing SPARQL servers via the command line |
| [SparqlGui](UserGuide-Tools-SparqlGui) | GUI for experimenting with SPARQL queries using our in-memory SPARQL implementation |
| [Store Manager](UserGuide-Tools-Store-Manager) | GUI for working with and managing any supported Triple Store for which we have a [Storage Provider](UserGuide-Storage-Providers) |