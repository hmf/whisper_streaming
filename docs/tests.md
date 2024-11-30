
<!-- cSpell:ignore SETUPTOOLS, pyproject -->

whisper_online_server.py

--host
--port 
--warmup-file

--model distil-large-v3
--lan auto
--task transcribe
--backend faster-whisper
--vac
--vad
-l INFO
--model_cache_dir /mnt/ssd2/hmf/VSCodeProjects/faster-whisper/models
--language en 
--min-chunk-size 1


Default "large-v2" model:

```shell
$ python3 -m whisper_online_server --backend faster-whisper --model_cache_dir /mnt/ssd2/hmf/VSCodeProjects/faster-whisper/models -l INFO
```

Does not set compute device and compute type. The [default](/mnt/ssd2/hmf/VSCodeProjects/whisper_streaming/whisper_online.py#L119) is:

```python
model = WhisperModel(model_size_or_path, device="cuda", compute_type="float16", download_root=cache_dir)
```

Code was changed to test "float32" and "int8". Because I only have a compute capability 6.1 with 4GiB, I get out of memory errors (for both types).

Lets try a smaller model:

```shell
$ python3 -m whisper_online_server --backend faster-whisper --model distil-large-v3 --model_cache_dir /mnt/ssd2/hmf/VSCodeProjects/faster-whisper/models -l INFO
```

Now I get the error:

```
whisper_online_server.py: error: argument --model: invalid choice: 'distil-large-v3' (choose from 'tiny.en', 'tiny', 'base.en', 'base', 'small.en', 'small', 'medium.en', 'medium', 'large-v1', 'large-v2', 'large-v3', 'large', 'large-v3-turbo')
```

```shell
$ python3 -m whisper_online_server --backend faster-whisper --model distil-large-v3 --model_cache_dir /mnt/ssd2/hmf/VSCodeProjects/faster-whisper/models -l INFO


```shell
arecord -f S16_LE -c1 -r 16000 -t raw -D default | nc localhost 43007
```



```
whisper_online.py -h

usage: whisper_online.py [-h] [--min-chunk-size MIN_CHUNK_SIZE] [--model {tiny.en,tiny,base.en,base,small.en,small,medium.en,medium,large-v1,large-v2,large-v3,large}] [--model_cache_dir MODEL_CACHE_DIR]
                         [--model_dir MODEL_DIR] [--lan LAN] [--task {transcribe,translate}] [--backend {faster-whisper,whisper_timestamped,openai-api}] [--vac] [--vac-chunk-size VAC_CHUNK_SIZE] [--vad]
                         [--buffer_trimming {sentence,segment}] [--buffer_trimming_sec BUFFER_TRIMMING_SEC] [-l {DEBUG,INFO,WARNING,ERROR,CRITICAL}] [--start_at START_AT] [--offline] [--comp_unaware]
                         audio_path

positional arguments:
  audio_path            Filename of 16kHz mono channel wav, on which live streaming is simulated.

options:
  -h, --help            show this help message and exit
  --min-chunk-size MIN_CHUNK_SIZE
                        Minimum audio chunk size in seconds. It waits up to this time to do processing. If the processing takes shorter time, it waits, otherwise it processes the whole segment that was
                        received by this time.
  --model {tiny.en,tiny,base.en,base,small.en,small,medium.en,medium,large-v1,large-v2,large-v3,large,large-v3-turbo}
                        Name size of the Whisper model to use (default: large-v2). The model is automatically downloaded from the model hub if not present in model cache dir.
  --model_cache_dir MODEL_CACHE_DIR
                        Overriding the default model cache dir where models downloaded from the hub are saved
  --model_dir MODEL_DIR
                        Dir where Whisper model.bin and other files are saved. This option overrides --model and --model_cache_dir parameter.
  --lan LAN, --language LAN
                        Source language code, e.g. en,de,cs, or 'auto' for language detection.
  --task {transcribe,translate}
                        Transcribe or translate.
  --backend {faster-whisper,whisper_timestamped,openai-api}
                        Load only this backend for Whisper processing.
  --vac                 Use VAC = voice activity controller. Recommended. Requires torch.
  --vac-chunk-size VAC_CHUNK_SIZE
                        VAC sample size in seconds.
  --vad                 Use VAD = voice activity detection, with the default parameters.
  --buffer_trimming {sentence,segment}
                        Buffer trimming strategy -- trim completed sentences marked with punctuation mark and detected by sentence segmenter, or the completed segments returned by Whisper. Sentence segmenter
                        must be installed for "sentence" option.
  --buffer_trimming_sec BUFFER_TRIMMING_SEC
                        Buffer trimming length threshold in seconds. If buffer length is longer, trimming sentence/segment is triggered.
  -l {DEBUG,INFO,WARNING,ERROR,CRITICAL}, --log-level {DEBUG,INFO,WARNING,ERROR,CRITICAL}
                        Set the log level
  --start_at START_AT   Start processing audio at this time.
  --offline             Offline mode.
  --comp_unaware        Computationally unaware simulation.
```