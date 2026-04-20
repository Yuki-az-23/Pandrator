# Pandrator Dry-Run Mode

## What is Dry-Run Mode?

Dry-run mode allows you to preview what Pandrator would do **without actually generating any TTS audio**. This is useful for:

- Testing your setup and configuration
- Previewing text preprocessing and sentence splitting
- Seeing what parameters would be used for TTS generation
- Debugging issues without waiting for audio generation
- Verifying your workflow before committing to a full generation run

## How to Use Dry-Run Mode

### Basic Usage

```bash
conda activate pandrator_installer
cd Pandrator
python pandrator.py --dry-run
```

### With Auto-Connect to XTTS

```bash
python pandrator.py --dry-run -connect -xtts
```

### With Auto-Connect to Silero

```bash
python pandrator.py --dry-run -connect -silero
```

## What Happens in Dry-Run Mode?

When dry-run mode is enabled:

1. **Window Title**: The application window will show `[DRY-RUN MODE]` in the title
2. **Status Message**: You'll see a status message indicating dry-run mode is active
3. **TTS Generation**: Instead of making actual API calls to generate audio:
   - The system logs what text would be sent for TTS generation
   - It logs which TTS service, language, and speaker would be used
   - It returns silent 1-second audio segments as placeholders
4. **Console Output**: Detailed logging shows exactly what would happen, for example:
   ```
   [DRY-RUN] Would generate TTS for: 'This is a sample sentence to be converted...'
   [DRY-RUN] TTS Service: XTTS
   [DRY-RUN] Language: en
   [DRY-RUN] Speaker: sample_voice.wav
   ```

## What Still Works Normally?

In dry-run mode, these features still work normally:

- File loading and text extraction (PDF, EPUB, TXT, etc.)
- Text preprocessing and sentence splitting
- Session management
- Settings configuration
- File browsing and selection

## What Doesn't Work?

In dry-run mode, these features are simulated or disabled:

- Actual TTS audio generation (simulated with silent audio)
- API calls to XTTS/Silero servers
- Audio quality evaluation (NISQA)
- RVC voice conversion
- Final audio file output

## Example Workflow

1. Start Pandrator in dry-run mode:
   ```bash
   python pandrator.py --dry-run -connect -xtts
   ```

2. Load a text file or PDF

3. Watch the console output to see:
   - How the text is being preprocessed
   - What sentences are being created
   - What parameters would be used for TTS

4. Review the logs in the `logs/` directory for detailed information

5. When satisfied with the preview, restart without `--dry-run` to perform actual generation

## Tips

- Use dry-run mode to test new configurations before long generation runs
- Check the logs directory for complete session information
- Dry-run mode is perfect for debugging text preprocessing issues
- All logging is still written to `logs/pandrator_TIMESTAMP.log`

## All Available Command-Line Options

```bash
python pandrator.py [OPTIONS]

Options:
  -h, --help          Show help message
  --dry-run           Preview mode - no actual TTS generation
  -connect            Auto-connect to TTS service on launch
  -xtts               Use with -connect to connect to XTTS
  -silero             Use with -connect to connect to Silero

Examples:
  python pandrator.py
  python pandrator.py --dry-run
  python pandrator.py -connect -xtts
  python pandrator.py --dry-run -connect -silero
```
