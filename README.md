# OutsourceTracker Shared .NET Workflows

This repository hosts reusable GitHub Actions workflows for .NET projects, promoting consistent build processes across multiple repositories. It's designed for projects like outsourcing tracking applications built with .NET 10, including Blazor WebAssembly frontends and Web API backends. Workflows here can handle building, testing, and publishing .NET components, with support for Bootstrap-integrated UIs and authentication via Microsoft 365, Microsoft, or Google (secrets handled securely).

## Purpose
- Centralize build logic to avoid duplication in your .NET repositories.
- Support .NET 10 SDK for modern features.
- Enable easy integration for Blazor WebAssembly publishing, ensuring responsive designs with Bootstrap CSS (using div-based tables for multi-screen compatibility).
- Facilitate secure handling of secrets, such as NuGet tokens or auth keys for Microsoft/Google services.

## Available Workflows
- **build-dotnet.yml**: A reusable workflow for building, testing, and optionally publishing .NET projects.
  - **Inputs**:
    - `dotnet-version`: .NET SDK version (default: '10.0.x').
    - `project-path`: Path to the .csproj file (required).
    - `publish-blazor`: Boolean to enable Blazor WebAssembly publish (default: false).
  - **Secrets**:
    - `nuget-token`: For private NuGet packages.
  - **Outputs**: Build artifacts uploaded for further use (e.g., deployment).

## Usage
To use these workflows in your project repositories:

1. **Reference the Workflow**:
   In your project's `.github/workflows/` directory, create a YAML file (e.g., `ci.yml`) and call the reusable workflow:

   ```yaml
   name: CI Pipeline

   on:
     push:
       branches: [ main ]
     pull_request:
       branches: [ main ]

   jobs:
     build-frontend:
       uses: yourusername/shared-dotnet-workflows/.github/workflows/build-dotnet.yml@main
       with:
         dotnet-version: '10.0.x'
         project-path: 'src/BlazorFrontend.csproj'
         publish-blazor: true
       secrets:
         nuget-token: ${{ secrets.NUGET_TOKEN }}

     build-backend:
       uses: yourusername/shared-dotnet-workflows/.github/workflows/build-dotnet.yml@main
       with:
         dotnet-version: '10.0.x'
         project-path: 'src/ApiBackend.csproj'
         publish-blazor: false
       secrets:
         inherit
	```