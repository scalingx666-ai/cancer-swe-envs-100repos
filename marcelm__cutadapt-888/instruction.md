# Issue

**Title:** Bump the github-actions group with 2 updates

## Description

Bumps the github-actions group with 2 updates: [actions/setup-python](https://github.com/actions/setup-python) and [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel).

Updates `actions/setup-python` from 6 to 7
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/setup-python/releases">actions/setup-python's releases</a>.</em></p>
<blockquote>
<h2>v7.0.0</h2>
<h2>What's Changed</h2>
<h3>Enhancements</h3>
<ul>
<li>Migrate to ESM and upgrade dependencies by <a href="https://github.com/priyagupta108"><code>@​priyagupta108</code></a> in <a href="">actions/setup-python#1330</a></li>
<li>Pin SHA commits and update docs with latest versions by <a href="https://github.com/HarithaVattikuti"><code>@​HarithaVattikuti</code></a> in <a href="">actions/setup-python#1338</a></li>
<li>Remove the pip-install input by <a href="https://github.com/gowridurgad"><code>@​gowridurgad</code></a> in <a href="">actions/setup-python#1336</a></li>
</ul>
<h3>Bug Fix</h3>
<ul>
<li>Fix to Classify stderr warning messages as warnings instead of errors in annotations by <a href="https://github.com/lmvysakh"><code>@​lmvysakh</code></a> in <a href="">actions/setup-python#1335</a></li>
<li>Validate and retry manifest fetch to prevent silent failures by <a href="https://github.com/priyagupta108"><code>@​priyagupta108</code></a> in <a href="">actions/setup-python#1332</a></li>
</ul>
<h3>Dependency Upgrade</h3>
<ul>
<li>Bump certifi from 2020.6.20 to 2024.7.4 in /<strong>tests</strong>/data by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1328</a></li>
<li>Remove EOL Python versions and Bumps numpy text fixture by <a href="https://github.com/priya-kinthali"><code>@​priya-kinthali</code></a> in <a href="">actions/setup-python#1333</a></li>
<li>Upgrade <code>@​actions/cache</code> to 6.2.0 by <a href="https://github.com/philip-gai"><code>@​philip-gai</code></a> in <a href="">actions/setup-python#1337</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/lmvysakh"><code>@​lmvysakh</code></a> made their first contribution in <a href="">actions/setup-python#1335</a></li>
<li><a href="https://github.com/philip-gai"><code>@​philip-gai</code></a> made their first contribution in <a href="">actions/setup-python#1337</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v6...v7.0.0">https://github.com/actions/setup-python/compare/v6...v7.0.0</a></p>
<h2>v6.3.0</h2>
<h2>What's Changed</h2>
<h3>Enhancement</h3>
<ul>
<li>Add RHEL support and include Linux distro in cache keys by <a href="https://github.com/priyagupta108"><code>@​priyagupta108</code></a> in <a href="">actions/setup-python#1323</a></li>
<li>Fix pip cache error handling on Windows by <a href="https://github.com/priyagupta108"><code>@​priyagupta108</code></a> in <a href="">actions/setup-python#1040</a></li>
</ul>
<h3>Dependency update</h3>
<ul>
<li>Upgrade minimatch from 3.1.2 to 3.1.5 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1281</a></li>
<li>Upgrade actions dependencies by <a href="https://github.com/gowridurgad"><code>@​gowridurgad</code></a> with <a href="https://github.com/Copilot"><code>@​Copilot</code></a> in <a href="">actions/setup-python#1303</a></li>
<li>Upgrade <code>@​actions/cache</code> to 5.1.0, log cache write denied by <a href="https://github.com/jasongin"><code>@​jasongin</code></a> in <a href="">actions/setup-python#1324</a></li>
<li>Upgrade dependency versions and test workflow configuration by <a href="https://github.com/HarithaVattikuti"><code>@​HarithaVattikuti</code></a> in <a href="">actions/setup-python#1322</a></li>
</ul>
<h3>Documentation</h3>
<ul>
<li>Update advanced-usage.md by <a href="https://github.com/Dunky-Z"><code>@​Dunky-Z</code></a> in <a href="">actions/setup-python#811</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/gowridurgad"><code>@​gowridurgad</code></a> with <a href="https://github.com/Copilot"><code>@​Copilot</code></a> made their first contribution in <a href="">actions/setup-python#1303</a></li>
<li><a href="https://github.com/jasongin"><code>@​jasongin</code></a> made their first contribution in <a href="">actions/setup-python#1324</a></li>
<li><a href="https://github.com/Dunky-Z"><code>@​Dunky-Z</code></a> made their first contribution in <a href="">actions/setup-python#811</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v6.2.0...v6.3.0">https://github.com/actions/setup-python/compare/v6.2.0...v6.3.0</a></p>
<h2>v6.2.0</h2>
<h2>What's Changed</h2>
<h3>Dependency Upgrades</h3>
<ul>
<li>Upgrade dependencies to Node 24 compatible versions by <a href="https://github.com/salmanmkc"><code>@​salmanmkc</code></a> in <a href="">actions/setup-python#1259</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>5fda3b9</code></a> Pin SHA commits and update docs with latest versions (<a href="">#1338</a>)</li>
<li><a href=""><code>4ab7e95</code></a> Merge pull request <a href="">#1337</a> from actions/philip-gai/bump-actions-cache-6-2-0</li>
<li><a href=""><code>0f3a009</code></a> Remove the pip-install input (<a href="">#1336</a>)</li>
<li><a href=""><code>f8cf429</code></a> Migrate to ESM and upgrade dependencies (<a href="">#1330</a>)</li>
<li><a href=""><code>54baeea</code></a> Validate and retry manifest fetch to prevent silent failures (<a href="">#1332</a>)</li>
<li><a href=""><code>c709277</code></a> Annotation code fix (<a href="">#1335</a>)</li>
<li><a href=""><code>6849080</code></a> remove EOL Python versions and Bumps numpy text fixture (<a href="">#1333</a>)</li>
<li><a href=""><code>0903b46</code></a> Bump certifi from 2020.6.20 to 2024.7.4 in /<strong>tests</strong>/data (<a href="">#1328</a>)</li>
<li>See full diff in <a href="https://github.com/actions/setup-python/compare/v6...v7">compare view</a></li>
</ul>
</details>
<br />

Updates `pypa/cibuildwheel` from 4.1.0 to 4.1.1
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>v4.1.1</h2>
<ul>
<li>✨ Adds <code>pyodide-build</code> as a separate <a href="https://cibuildwheel.pypa.io/en/stable/options/#build-frontend"><code>build-frontend</code></a>, now the default frontend for Pyodide, with verbosity flags handling. Any other frontend is ignored with a warning on Pyodide. (<a href="">#2609</a>, <a href="">#2945</a>)</li>
<li>🔐 Uses digests instead of tags for pinned container images, strengthening supply-chain security. The human-readable tags remain as comments in <code>pinned_docker_images.cfg</code>. (<a href="">#2915</a>)</li>
<li>🐛 Fixes platform-specific <a href="https://cibuildwheel.pypa.io/en/stable/options/#test-runtime"><code>test-runtime</code></a> environment variables (e.g. <code>CIBW_TEST_RUNTIME_ANDROID</code>) not being honored (<a href="">#2941</a>)</li>
<li>🐛 Fixes quoting of <a href="https://cibuildwheel.pypa.io/en/stable/options/#test-requires"><code>test-requires</code></a> and <a href="https://cibuildwheel.pypa.io/en/stable/options/#audit-requires"><code>audit-requires</code></a> so PEP 508 specifiers containing spaces work (<a href="">#2913</a>)</li>
<li>🐛 Makes <a href="https://cibuildwheel.pypa.io/en/stable/options/#archs"><code>archs</code></a> parsing case-insensitive and platform-aware, so e.g. <code>arm64</code> works on Windows (<a href="">#2920</a>)</li>
<li>🐛 Uses an absolute path for the <code>{project}</code> placeholder in <a href="https://cibuildwheel.pypa.io/en/stable/options/#config-settings"><code>config-settings</code></a> (<a href="">#2934</a>)</li>
<li>🐛 Validates the <a href="https://cibuildwheel.pypa.io/en/stable/options/#pyodide-version"><code>pyodide-version</code></a> option against the build identifier with a clear error (<a href="">#2925</a>)</li>
<li>🐛 Fixes PyPy installs on macOS after PyPy switched its downloads from <code>.tar.bz2</code> to <code>.tar.gz</code> (<a href="">#2939</a>)</li>
<li>🐛 Makes a matching <code>python3-config</code> available in the build and test venvs on macOS (<a href="">#2922</a>)</li>
<li>🛠 Updates dependencies and container pins (<a href="">#2917</a>, <a href="">#2935</a>, <a href="">#2939</a>)</li>
<li>🛠 Updates Android tests to current Python versions and the new test repository URL (<a href="">#2933</a>)</li>
<li>🛠 Drops the <code>orjson</code> dependency, no longer used by mypy 2+ (<a href="">#2923</a>)</li>
<li>📚 Builds the docs with properdocs, a MkDocs fork (<a href="">#2946</a>)</li>
<li>📚 Adds the missing <code>cp314-pyodide_wasm32</code> entry to the build identifier table (<a href="">#2947</a>)</li>
<li>📚 Removes outdated notes about the <code>pip wheel</code> build frontend and ClearLinux (<a href="">#2926</a>)</li>
<li>💼 Adds a &quot;CI: PyPy EoL&quot; PR label to run PyPy EoL tests on PRs (<a href="">#2930</a>)</li>
<li>💼 Updates CI action pins and pre-commit hooks (<a href="">#2914</a>, <a href="">#2932</a>, <a href="">#2938</a>, <a href="">#2940</a>, <a href="">#2942</a>, <a href="">#2943</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<h3>v4.1.1</h3>
<p><em>24 July 2026</em></p>
<ul>
<li>✨ Adds <code>pyodide-build</code> as a separate <a href="https://cibuildwheel.pypa.io/en/stable/options/#build-frontend"><code>build-frontend</code></a>, now the default frontend for Pyodide, with verbosity flags handling. Any other frontend is ignored with a warning on Pyodide. (<a href="">#2609</a>, <a href="">#2945</a>)</li>
<li>🔐 Uses digests instead of tags for pinned container images, strengthening supply-chain security. The human-readable tags remain as comments in <code>pinned_docker_images.cfg</code>. (<a href="">#2915</a>)</li>
<li>🐛 Fixes platform-specific <a href="https://cibuildwheel.pypa.io/en/stable/options/#test-runtime"><code>test-runtime</code></a> environment variables (e.g. <code>CIBW_TEST_RUNTIME_ANDROID</code>) not being honored (<a href="">#2941</a>)</li>
<li>🐛 Fixes quoting of <a href="https://cibuildwheel.pypa.io/en/stable/options/#test-requires"><code>test-requires</code></a> and <a href="https://cibuildwheel.pypa.io/en/stable/options/#audit-requires"><code>audit-requires</code></a> so PEP 508 specifiers containing spaces work (<a href="">#2913</a>)</li>
<li>🐛 Makes <a href="https://cibuildwheel.pypa.io/en/stable/options/#archs"><code>archs</code></a> parsing case-insensitive and platform-aware, so e.g. <code>arm64</code> works on Windows (<a href="">#2920</a>)</li>
<li>🐛 Uses an absolute path for the <code>{project}</code> placeholder in <a href="https://cibuildwheel.pypa.io/en/stable/options/#config-settings"><code>config-settings</code></a> (<a href="">#2934</a>)</li>
<li>🐛 Validates the <a href="https://cibuildwheel.pypa.io/en/stable/options/#pyodide-version"><code>pyodide-version</code></a> option against the build identifier with a clear error (<a href="">#2925</a>)</li>
<li>🐛 Fixes PyPy installs on macOS after PyPy switched its downloads from <code>.tar.bz2</code> to <code>.tar.gz</code> (<a href="">#2939</a>)</li>
<li>🐛 Makes a matching <code>python3-config</code> available in the build and test venvs on macOS (<a href="">#2922</a>)</li>
<li>🛠 Updates dependencies and container pins (<a href="">#2917</a>, <a href="">#2935</a>, <a href="">#2939</a>)</li>
<li>🛠 Updates Android tests to current Python versions and the new test repository URL (<a href="">#2933</a>)</li>
<li>🛠 Drops the <code>orjson</code> dependency, no longer used by mypy 2+ (<a href="">#2923</a>)</li>
<li>📚 Builds the docs with properdocs, a MkDocs fork (<a href="">#2946</a>)</li>
<li>📚 Adds the missing <code>cp314-pyodide_wasm32</code> entry to the build identifier table (<a href="">#2947</a>)</li>
<li>📚 Removes outdated notes about the <code>pip wheel</code> build frontend and ClearLinux (<a href="">#2926</a>)</li>
<li>💼 Adds a &quot;CI: PyPy EoL&quot; PR label to run PyPy EoL tests on PRs (<a href="">#2930</a>)</li>
<li>💼 Updates CI action pins and pre-commit hooks (<a href="">#2914</a>, <a href="">#2932</a>, <a href="">#2938</a>, <a href="">#2940</a>, <a href="">#2942</a>, <a href="">#2943</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>4726cd3</code></a> Bump version: v4.1.1</li>
<li><a href=""><code>1af5cd7</code></a> docs: switch from mkdocs to properdocs (<a href="">#2946</a>)</li>
<li><a href=""><code>b21d76a</code></a> docs: add missing cp314-pyodide_wasm32 to build-id table (<a href="">#2947</a>)</li>
<li><a href=""><code>03cbe93</code></a> fix(macos): make matching python3-config available in build/test venvs (<a href="">#2922</a>)</li>
<li><a href=""><code>b7cac6d</code></a> fix: don't error when a global build-frontend is set on pyodide (<a href="">#2945</a>)</li>
<li><a href=""><code>1520daf</code></a> fix: support platform-specific test runtime environment variables (<a href="">#2941</a>)</li>
<li><a href=""><code>17b7420</code></a> [Bot] Update dependencies (<a href="">#2939</a>)</li>
<li><a href=""><code>f8d8cca</code></a> chore(deps): bump astral-sh/setup-uv from 8.2.0 to 8.3.2 in the actions group...</li>
<li><a href=""><code>265b435</code></a> chore(deps): bump the actions group with 2 updates (<a href="">#2942</a>)</li>
<li><a href=""><code>3f8bd1c</code></a> chore(deps): bump the actions group with 3 updates (<a href="">#2938</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/v4.1.0...v4.1.1">compare view</a></li>
</ul>
</details>
<br />

Dependabot will resolve any conflicts with this PR as long as you don't alter it yourself. You can also trigger a rebase manually by commenting `@dependabot rebase`.

[//]: # (dependabot-automerge-start)
[//]: # (dependabot-automerge-end)

---

<details>
<summary>Dependabot commands and options</summary>
<br />

You can trigger Dependabot actions by commenting on this PR:
- `@dependabot rebase` will rebase this PR
- `@dependabot recreate` will recreate this PR, overwriting any edits that have been made to it
- `@dependabot show <dependency name> ignore conditions` will show all of the ignore conditions of the specified dependency
- `@dependabot ignore <dependency name> major version` will close this group update PR and stop Dependabot creating any more for the specific dependency's major version (unless you unignore this specific dependency's major version or upgrade to it yourself)
- `@dependabot ignore <dependency name> minor version` will close this group update PR and stop Dependabot creating any more for the specific dependency's minor version (unless you unignore this specific dependency's minor version or upgrade to it yourself)
- `@dependabot ignore <dependency name>` will close this group update PR and stop Dependabot creating any more for the specific dependency (unless you unignore this specific dependency or upgrade to it yourself)
- `@dependabot unignore <dependency name>` will remove all of the ignore conditions of the specified dependency
- `@dependabot unignore <dependency name> <ignore condition>` will remove the ignore condition of the specified dependency and ignore conditions

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50e9fb8d35b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.