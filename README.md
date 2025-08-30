# Complete Halon Build Instructions

## Repository Information
The official Mozilla Firefox repository is at `https://github.com/mozilla-firefox/firefox`.

## System Requirements

### Hardware Requirements
- **Memory**: 4GB RAM minimum, 8GB+ recommended
- **Disk Space**: At least 30-40GB of free disk space
- **CPU**: 64-bit processor
- **Network**: Fast internet connection (initial download is several GB)

### Operating System Support
- **Linux**: 64-bit installation (Ubuntu, Fedora, etc.)
- **Windows**: Windows 10 or later
- **macOS**: Recent versions supported

## Build Instructions by Platform

### Linux Build Instructions

#### 1. System Preparation
Install required dependencies based on your distribution:

**For Debian/Ubuntu-based systems:**
```bash
sudo apt update && sudo apt install curl python3 python3-pip git
```

**For Fedora/RHEL-based systems:**
```bash
sudo dnf install python3 python3-pip git
```

#### 2. Download and Bootstrap
Download the bootstrap script from the correct repository and run the setup:
```bash
curl -LO https://raw.githubusercontent.com/mozilla-firefox/firefox/refs/heads/main/python/mozboot/bin/bootstrap.py
python3 bootstrap.py
```

The bootstrap process will:
- Clone the Firefox source code (mozilla-central)
- Download additional build dependencies
- Guide you through an interactive setup process

#### 3. Choose Build Type
During bootstrap, you'll be prompted to choose a build type:

**Artifact Build (Recommended for most users):**
- Downloads pre-compiled Firefox components
- Faster builds, less disk space
- Good for frontend development, add-ons, or minor modifications
- Choose "Firefox for Desktop Artifact Mode"

**Full Build:**
- Compiles everything from source
- Required for backend/engine modifications
- Takes significantly longer (1-2 hours initial build)
- Choose "Firefox for Desktop"

#### 4. Build Firefox
Navigate to the source directory and build:
```bash
cd firefox  # or mozilla-central, depending on setup
git pull    # Get latest changes
./mach build
```

#### 5. Run Your Build
After successful compilation:
```bash
./mach run
```

### Windows Build Instructions

#### 1. Prerequisites
- **Operating System**: Windows 10 or later, fully updated
- **Memory**: 4GB minimum, 8GB+ recommended  
- **Disk Space**: At least 40GB free
- **Visual Studio**: Install Visual Studio Community (free) with C++ development tools

#### 2. Setup Development Environment
1. Download and install [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/)
2. During installation, select "Desktop development with C++"
3. Install [Git for Windows](https://git-scm.com/download/win)
4. Ensure Python 3.8+ is installed

#### 3. Bootstrap Process
Open Command Prompt or PowerShell as Administrator:
```cmd
curl -LO https://raw.githubusercontent.com/mozilla-firefox/firefox/refs/heads/main/python/mozboot/bin/bootstrap.py
python bootstrap.py
```

#### 4. Build
```cmd
cd firefox
mach build
```

#### 5. Run
```cmd
mach run
```

### macOS Build Instructions

#### 1. Prerequisites
- **Xcode**: Install from App Store (includes command line tools)
- **Homebrew**: Install package manager
- **Memory**: 4GB minimum, 8GB+ recommended

#### 2. Install Dependencies
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install required tools
brew install python3 git
```

#### 3. Bootstrap and Build
```bash
curl -LO https://raw.githubusercontent.com/mozilla-firefox/firefox/refs/heads/main/python/mozboot/bin/bootstrap.py
python3 bootstrap.py
cd firefox
./mach build
./mach run
```

## Build Configuration Options

### Mozconfig File
You can customize builds using a `.mozconfig` file in the source root. Common options:

```bash
# Debug build (slower but includes debugging symbols)
ac_add_options --enable-debug

# Optimized build (default for releases)
ac_add_options --enable-optimize

# Artifact build
ac_add_options --enable-artifact-builds

# Custom branding
ac_add_options --enable-official-branding
```

## Post-Build Information

### Output Location
- **Linux/macOS**: `obj-*/dist/bin/firefox`
- **Windows**: `obj-*/dist/bin/firefox.exe`

### Running Tests
```bash
./mach test
./mach test path/to/specific/test
```

### Creating Packages
```bash
# Create installable package
./mach package

# Packages will be in obj-*/dist/
```

## Troubleshooting Common Issues

### Build Errors
1. **"CLOBBER file updated"**: Run `./mach clobber` then rebuild
2. **Out of disk space**: Ensure 30-40GB free space
3. **Memory issues**: Close other applications, consider artifact builds
4. **Dependencies missing**: Re-run `python3 bootstrap.py`

### Performance Tips
- Use artifact builds for faster compilation
- Enable ccache for incremental builds: `ac_add_options --with-ccache`
- Use `./mach build faster` for optimized incremental builds

### Getting Help
- **Matrix Chat**: Join [#introduction:mozilla.org](https://chat.mozilla.org/#/room/#introduction:mozilla.org)
- **Bugzilla**: [bugzilla.mozilla.org](https://bugzilla.mozilla.org/)
- **Documentation**: [Firefox Source Docs](https://firefox-source-docs.mozilla.org/)

## Development Workflow

### Making Changes
1. Create a new branch: `git checkout -b my-feature-branch`
2. Make your changes
3. Build and test: `./mach build && ./mach run`
4. Run relevant tests: `./mach test`

### Submitting Changes
1. Commit changes: `git commit -am "Description of changes"`
2. Create a patch or pull request through Mozilla's review system
3. Follow [Mozilla's contribution guidelines](https://firefox-source-docs.mozilla.org/contributing/)

## Final Notes

- Initial builds can take 1-4 hours depending on hardware and build type
- Subsequent incremental builds are much faster (5-30 minutes)
- Artifact builds significantly reduce build times
- Keep your source tree updated with `git pull` regularly
- Join the Mozilla community for support and collaboration

The end result will be a fully functional Firefox browser executable that you can run, test, and distribute (following Mozilla's licensing terms).
