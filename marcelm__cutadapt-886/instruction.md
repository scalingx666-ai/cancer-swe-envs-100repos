# Issue

**Title:** Bump the github-actions group across 1 directory with 3 updates

## Description

Bumps the github-actions group with 3 updates in the / directory: [actions/checkout](https://github.com/actions/checkout), [codecov/codecov-action](https://github.com/codecov/codecov-action) and [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel).

Updates `actions/checkout` from 6 to 7
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/checkout/releases">actions/checkout's releases</a>.</em></p>
<blockquote>
<h2>v7.0.0</h2>
<h2>What's Changed</h2>
<ul>
<li>block checking out fork pr for pull_request_target and workflow_run by <a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> in <a href="">actions/checkout#2454</a></li>
<li>Bump actions/publish-immutable-action from 0.0.3 to 0.0.4 in the minor-actions-dependencies group across 1 directory by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2458</a></li>
<li>Bump flatted from 3.3.1 to 3.4.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2460</a></li>
<li>Bump js-yaml from 4.1.0 to 4.2.0 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2461</a></li>
<li>Bump <code>@​actions/core</code> and <code>@​actions/tool-cache</code> and Remove uuid by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2459</a></li>
<li>upgrade module to esm and update dependencies by <a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> in <a href="">actions/checkout#2463</a></li>
<li>Bump the minor-npm-dependencies group across 1 directory with 3 updates by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2462</a></li>
<li>getting ready for checkout v7 release by <a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> in <a href="">actions/checkout#2464</a></li>
<li>update error wording by <a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> in <a href="">actions/checkout#2467</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> made their first contribution in <a href="">actions/checkout#2454</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v6.0.3...v7.0.0">https://github.com/actions/checkout/compare/v6.0.3...v7.0.0</a></p>
<h2>v6.0.3</h2>
<h2>What's Changed</h2>
<ul>
<li>Update changelog by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2357</a></li>
<li>fix: expand merge commit SHA regex and add SHA-256 test cases by <a href="https://github.com/yaananth"><code>@​yaananth</code></a> in <a href="">actions/checkout#2414</a></li>
<li>Fix checkout init for SHA-256 repositories by <a href="https://github.com/yaananth"><code>@​yaananth</code></a> in <a href="">actions/checkout#2439</a></li>
<li>Update changelog for v6.0.3 by <a href="https://github.com/yaananth"><code>@​yaananth</code></a> in <a href="">actions/checkout#2446</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/yaananth"><code>@​yaananth</code></a> made their first contribution in <a href="">actions/checkout#2414</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v6...v6.0.3">https://github.com/actions/checkout/compare/v6...v6.0.3</a></p>
<h2>v6.0.2</h2>
<h2>What's Changed</h2>
<ul>
<li>Add orchestration_id to git user-agent when ACTIONS_ORCHESTRATION_ID is set by <a href="https://github.com/TingluoHuang"><code>@​TingluoHuang</code></a> in <a href="">actions/checkout#2355</a></li>
<li>Fix tag handling: preserve annotations and explicit fetch-tags by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2356</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v6.0.1...v6.0.2">https://github.com/actions/checkout/compare/v6.0.1...v6.0.2</a></p>
<h2>v6.0.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Update all references from v5 and v4 to v6 by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2314</a></li>
<li>Add worktree support for persist-credentials includeIf by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2327</a></li>
<li>Clarify v6 README by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2328</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/checkout/compare/v6...v6.0.1">https://github.com/actions/checkout/compare/v6...v6.0.1</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/actions/checkout/blob/main/CHANGELOG.md">actions/checkout's changelog</a>.</em></p>
<blockquote>
<h1>Changelog</h1>
<h2>v7.0.0</h2>
<ul>
<li>Block checking out fork PR for pull_request_target and workflow_run by <a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> in <a href="">actions/checkout#2454</a></li>
<li>Bump actions/publish-immutable-action from 0.0.3 to 0.0.4 in the minor-actions-dependencies group across 1 directory by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2458</a></li>
<li>Bump flatted from 3.3.1 to 3.4.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2460</a></li>
<li>Bump js-yaml from 4.1.0 to 4.2.0 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2461</a></li>
<li>Bump <code>@​actions/core</code> and <code>@​actions/tool-cache</code> and Remove uuid by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2459</a></li>
<li>upgrade module to esm and update dependencies by <a href="https://github.com/aiqiaoy"><code>@​aiqiaoy</code></a> in <a href="">actions/checkout#2463</a></li>
<li>Bump the minor-npm-dependencies group across 1 directory with 3 updates by <a href="https://github.com/dependabot"><code>@​dependabot</code></a>[bot] in <a href="">actions/checkout#2462</a></li>
</ul>
<h2>v6.0.3</h2>
<ul>
<li>Fix checkout init for SHA-256 repositories by <a href="https://github.com/yaananth"><code>@​yaananth</code></a> in <a href="">actions/checkout#2439</a></li>
<li>fix: expand merge commit SHA regex and add SHA-256 test cases by <a href="https://github.com/yaananth"><code>@​yaananth</code></a> in <a href="">actions/checkout#2414</a></li>
</ul>
<h2>v6.0.2</h2>
<ul>
<li>Fix tag handling: preserve annotations and explicit fetch-tags by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2356</a></li>
</ul>
<h2>v6.0.1</h2>
<ul>
<li>Add worktree support for persist-credentials includeIf by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2327</a></li>
</ul>
<h2>v6.0.0</h2>
<ul>
<li>Persist creds to a separate file by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2286</a></li>
<li>Update README to include Node.js 24 support details and requirements by <a href="https://github.com/salmanmkc"><code>@​salmanmkc</code></a> in <a href="">actions/checkout#2248</a></li>
</ul>
<h2>v5.0.1</h2>
<ul>
<li>Port v6 cleanup to v5 by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2301</a></li>
</ul>
<h2>v5.0.0</h2>
<ul>
<li>Update actions checkout to use node 24 by <a href="https://github.com/salmanmkc"><code>@​salmanmkc</code></a> in <a href="">actions/checkout#2226</a></li>
</ul>
<h2>v4.3.1</h2>
<ul>
<li>Port v6 cleanup to v4 by <a href="https://github.com/ericsciple"><code>@​ericsciple</code></a> in <a href="">actions/checkout#2305</a></li>
</ul>
<h2>v4.3.0</h2>
<ul>
<li>docs: update README.md by <a href="https://github.com/motss"><code>@​motss</code></a> in <a href="">actions/checkout#1971</a></li>
<li>Add internal repos for checking out multiple repositories by <a href="https://github.com/mouismail"><code>@​mouismail</code></a> in <a href="">actions/checkout#1977</a></li>
<li>Documentation update - add recommended permissions to Readme by <a href="https://github.com/benwells"><code>@​benwells</code></a> in <a href="">actions/checkout#2043</a></li>
<li>Adjust positioning of user email note and permissions heading by <a href="https://github.com/joshmgross"><code>@​joshmgross</code></a> in <a href="">actions/checkout#2044</a></li>
<li>Update README.md by <a href="https://github.com/nebuk89"><code>@​nebuk89</code></a> in <a href="">actions/checkout#2194</a></li>
<li>Update CODEOWNERS for actions by <a href="https://github.com/TingluoHuang"><code>@​TingluoHuang</code></a> in <a href="">actions/checkout#2224</a></li>
<li>Update package dependencies by <a href="https://github.com/salmanmkc"><code>@​salmanmkc</code></a> in <a href="">actions/checkout#2236</a></li>
</ul>
<h2>v4.2.2</h2>
<ul>
<li><code>url-helper.ts</code> now leverages well-known environment variables by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1941</a></li>
<li>Expand unit test coverage for <code>isGhes</code> by <a href="https://github.com/jww3"><code>@​jww3</code></a> in <a href="">actions/checkout#1946</a></li>
</ul>
<h2>v4.2.1</h2>
<ul>
<li>Check out other refs/* by commit if provided, fall back to ref by <a href="https://github.com/orhantoy"><code>@​orhantoy</code></a> in <a href="">actions/checkout#1924</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>9c091bb</code></a> update error wording (<a href="">#2467</a>)</li>
<li><a href=""><code>1044a6d</code></a> getting ready for checkout v7 release (<a href="">#2464</a>)</li>
<li><a href=""><code>f028218</code></a> Bump the minor-npm-dependencies group across 1 directory with 3 updates (<a href="">#2462</a>)</li>
<li><a href=""><code>d914b26</code></a> upgrade module to esm and update dependencies (<a href="">#2463</a>)</li>
<li><a href=""><code>537c7ef</code></a> Bump <code>@​actions/core</code> and <code>@​actions/tool-cache</code> and Remove uuid (<a href="">#2459</a>)</li>
<li><a href=""><code>130a169</code></a> Bump js-yaml from 4.1.0 to 4.2.0 (<a href="">#2461</a>)</li>
<li><a href=""><code>7d09575</code></a> Bump flatted from 3.3.1 to 3.4.2 (<a href="">#2460</a>)</li>
<li><a href=""><code>0f9f3aa</code></a> Bump actions/publish-immutable-action (<a href="">#2458</a>)</li>
<li><a href=""><code>f9e715a</code></a> block checking out fork pr for pull_request_target and workflow_run (<a href="">#2454</a>)</li>
<li>See full diff in <a href="https://github.com/actions/checkout/compare/v6...v7">compare view</a></li>
</ul>
</details>
<br />

Updates `codecov/codecov-action` from 6 to 7
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/codecov/codecov-action/releases">codecov/codecov-action's releases</a>.</em></p>
<blockquote>
<h2>v7.0.0</h2>
<p>⚠️ Due to migration issues with keybase, we are unable to update our keys under the <code>codecovsecurity</code> account. We have deleted the account and are using <code>codecovsecops</code> with the original gpg key</p>
<h2>What's Changed</h2>
<ul>
<li>ci: remove Enforce License Compliance workflow by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1950</a></li>
<li>chore(release): 7.0.0 by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1957</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v6.0.1...v7.0.0">https://github.com/codecov/codecov-action/compare/v6.0.1...v7.0.0</a></p>
<h2>v6.0.2</h2>
<p>This is a copy of the <code>v7.0.0</code> release to make updates easier</p>
<h2>What's Changed</h2>
<ul>
<li>ci: remove Enforce License Compliance workflow by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1950</a></li>
<li>chore(release): 7.0.0 by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1957</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v6.0.1...v6.0.2">https://github.com/codecov/codecov-action/compare/v6.0.1...v6.0.2</a></p>
<h2>v6.0.1</h2>
<h2>What's Changed</h2>
<ul>
<li>fix: prevent template injection in run: steps (VULN-1652) by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1947</a></li>
<li>chore(release): 6.0.1 by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1949</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v6.0.0...v6.0.1">https://github.com/codecov/codecov-action/compare/v6.0.0...v6.0.1</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/codecov/codecov-action/blob/main/CHANGELOG.md">codecov/codecov-action's changelog</a>.</em></p>
<blockquote>
<h2>v5.5.2</h2>
<h3>What's Changed</h3>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v5.5.1..v5.5.2">https://github.com/codecov/codecov-action/compare/v5.5.1..v5.5.2</a></p>
<h2>v5.5.1</h2>
<h3>What's Changed</h3>
<ul>
<li>fix: overwrite pr number on fork by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1871</a></li>
<li>build(deps): bump actions/checkout from 4.2.2 to 5.0.0 by <code>@​app/dependabot</code> in <a href="">codecov/codecov-action#1868</a></li>
<li>build(deps): bump github/codeql-action from 3.29.9 to 3.29.11 by <code>@​app/dependabot</code> in <a href="">codecov/codecov-action#1867</a></li>
<li>fix: update to use local app/ dir by <a href="https://github.com/thomasrockhu-codecov"><code>@​thomasrockhu-codecov</code></a> in <a href="">codecov/codecov-action#1872</a></li>
<li>docs: fix typo in README by <a href="https://github.com/datalater"><code>@​datalater</code></a> in <a href="">codecov/codecov-action#1866</a></li>
<li>Document a <code>codecov-cli</code> version reference example by <a href="https://github.com/webknjaz"><code>@​webknjaz</code></a> in <a href="">codecov/codecov-action#1774</a></li>
<li>build(deps): bump github/codeql-action from 3.28.18 to 3.29.9 by <code>@​app/dependabot</code> in <a href="">codecov/codecov-action#1861</a></li>
<li>build(deps): bump ossf/scorecard-action from 2.4.1 to 2.4.2 by <code>@​app/dependabot</code> in <a href="">codecov/codecov-action#1833</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v5.5.0..v5.5.1">https://github.com/codecov/codecov-action/compare/v5.5.0..v5.5.1</a></p>
<h2>v5.5.0</h2>
<h3>What's Changed</h3>
<ul>
<li>feat: upgrade wrapper to 0.2.4 by <a href="https://github.com/jviall"><code>@​jviall</code></a> in <a href="">codecov/codecov-action#1864</a></li>
<li>Pin actions/github-script by Git SHA by <a href="https://github.com/martincostello"><code>@​martincostello</code></a> in <a href="">codecov/codecov-action#1859</a></li>
<li>fix: check reqs exist by <a href="https://github.com/joseph-sentry"><code>@​joseph-sentry</code></a> in <a href="">codecov/codecov-action#1835</a></li>
<li>fix: Typo in README by <a href="https://github.com/spalmurray"><code>@​spalmurray</code></a> in <a href="">codecov/codecov-action#1838</a></li>
<li>docs: Refine OIDC docs by <a href="https://github.com/spalmurray"><code>@​spalmurray</code></a> in <a href="">codecov/codecov-action#1837</a></li>
<li>build(deps): bump github/codeql-action from 3.28.17 to 3.28.18 by <code>@​app/dependabot</code> in <a href="">codecov/codecov-action#1829</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v5.4.3..v5.5.0">https://github.com/codecov/codecov-action/compare/v5.4.3..v5.5.0</a></p>
<h2>v5.4.3</h2>
<h3>What's Changed</h3>
<ul>
<li>build(deps): bump github/codeql-action from 3.28.13 to 3.28.17 by <code>@​app/dependabot</code> in <a href="">codecov/codecov-action#1822</a></li>
<li>fix: OIDC on forks by <a href="https://github.com/joseph-sentry"><code>@​joseph-sentry</code></a> in <a href="">codecov/codecov-action#1823</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/codecov/codecov-action/compare/v5.4.2..v5.4.3">https://github.com/codecov/codecov-action/compare/v5.4.2..v5.4.3</a></p>
<h2>v5.4.2</h2>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li>See full diff in <a href="https://github.com/codecov/codecov-action/compare/v6...v7">compare view</a></li>
</ul>
</details>
<br />

Updates `pypa/cibuildwheel` from 3.4.0 to 4.1.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>v4.1.0</h2>
<ul>
<li>✨ Updates Pyodide to the final 314.0.0 release, so Pyodide 3.14 wheels now build by default without the <code>pyodide-prerelease</code> <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code></a> flag. (<a href="">#2906</a>)</li>
<li>🐛 Raises clear errors when a build produces no wheel, instead of failing later with a confusing message (<a href="">#2909</a>)</li>
<li>🛠 Speeds up CLI startup through lazy imports on Python 3.15 (<a href="">#2797</a>)</li>
<li>📚 Adds an FAQ section on caching cibuildwheel's downloaded tools with <code>CIBW_CACHE_PATH</code> (<a href="">#2842</a>)</li>
<li>📚 Documentation improvements: clarifies which shell is used for command options, clarifies environment variable precedence, and fixes a dead Pyodide env info link (<a href="">#2904</a>, <a href="">#2905</a>, <a href="">#2911</a>)</li>
</ul>
<h2>v4.0.0</h2>
<p>See <a href="https://github.com/henryiii"><code>@​henryiii</code></a>'s <a href="https://iscinumpy.dev/post/cibuildwheel-4-0-0/">release post</a> for more info on new features!</p>
<ul>
<li>
<p>🌟 Adds wheel auditing with <code>abi3audit</code> as a default after the repair step, with new <a href="https://cibuildwheel.pypa.io/en/stable/options/#audit-requires"><code>audit-requires</code></a> and <a href="https://cibuildwheel.pypa.io/en/stable/options/#audit-command"><code>audit-command</code></a> options (<a href="">#2805</a>)</p>
</li>
<li>
<p>🌟 Adds <code>pyemscripten</code> platform tag support (PEP 783), updates Pyodide to 314.0.0a2, and adds a <code>pyodide-eol</code> <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code></a> flag for building end-of-life Pyodide versions (<a href="">#2812</a>, <a href="">#2848</a>)</p>
</li>
<li>
<p>🌟 Sets up <code>delvewheel</code> as the default <a href="https://cibuildwheel.pypa.io/en/stable/options/#repair-wheel-command"><code>repair-wheel-command</code></a> for Windows, so extension module DLLs are now bundled automatically. Skip by setting it to empty if not needed. (<a href="">#2831</a>)</p>
</li>
<li>
<p>✨ Adds CPython 3.15 support, under the <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code> option</a> <code>cpython-prerelease</code>. This version of cibuildwheel uses 3.15.0b2. (<a href="">#2833</a>, <a href="">#2850</a>)</p>
<p><em>While CPython is in beta, the ABI can change, so your wheels might not be compatible with the final release. For this reason, we don't recommend distributing wheels until RC1, at which point 3.15 will be available in cibuildwheel without the flag.</em></p>
</li>
<li>
<p>✨ Adds CPython 3.15 support for iOS and Android (<a href="">#2857</a>, <a href="">#2858</a>)</p>
</li>
<li>
<p>✨ Adds Android improvements for building NumPy and related packages, including auditwheel support, pkg-config and Fortran configuration, and the <a href="https://cibuildwheel.pypa.io/en/stable/options/#xbuild-files"><code>xbuild-files</code></a> option (<a href="">#2695</a>)</p>
</li>
<li>
<p>✨ Adds <code>CIBUILDWHEEL_BUILD_IDENTIFIER</code> environment variable set to the current build identifier (e.g. <code>cp311-manylinux_x86_64</code>) during per-build steps (<a href="">#2872</a>)</p>
</li>
<li>
<p>✨ Adds <code>{project}</code> and <code>{package}</code> placeholders to <a href="https://cibuildwheel.pypa.io/en/stable/options/#config-settings"><code>config-settings</code></a> (<a href="">#2827</a>)</p>
</li>
<li>
<p>⚠️ Drops support for Python 3.8 (<a href="">#2686</a>)</p>
</li>
<li>
<p>⚠️ Removes the experimental CPython 3.13 free-threading builds and the <code>cpython-freethreading</code> <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code></a> option. CPython 3.14+ free-threading support remains available without the enable flag. (<a href="">#2684</a>)</p>
</li>
<li>
<p>⚠️ Drops support for Cirrus CI, which is shutting down June 1, 2026 (<a href="">#2817</a>)</p>
</li>
<li>
<p>⚠️ Drops GraalPy 3.11 (gp311) support, as agreed in <a href="">#2741</a>, and removes GraalPy 24-only workarounds (<a href="">#2895</a>)</p>
</li>
<li>
<p>🔐 Adds SHA256 verification for direct downloads of Python interpreters, virtualenv, and python-build-standalone assets (<a href="">#2873</a>)</p>
</li>
<li>
<p>🔐 Adds tarfile extraction filter for safe archive extraction (<a href="">#2856</a>)</p>
</li>
<li>
<p>🐛 Fixes <code>UV_PYTHON</code> not being set for <a href="https://cibuildwheel.pypa.io/en/stable/options/#before-build"><code>before-build</code></a> on Linux when using <code>uv</code> as the <a href="https://cibuildwheel.pypa.io/en/stable/options/#build-frontend"><code>build-frontend</code></a> (<a href="">#2830</a>)</p>
</li>
<li>
<p>🐛 Fixes detection of musl libc when downloading python-build-standalone, which previously always selected the gnu asset on musl hosts like Alpine (<a href="">#2889</a>)</p>
</li>
<li>
<p>🐛 Fixes <a href="https://cibuildwheel.pypa.io/en/stable/options/#config-settings"><code>config-settings</code></a> expansion when <code>{project}</code> or <code>{package}</code> contains spaces or backslashes (<a href="">#2886</a>)</p>
</li>
<li>
<p>🐛 Prevents deadlock when <code>linux32</code> fails and forwards platform args to the sanity check (<a href="">#2880</a>, <a href="">#2888</a>)</p>
</li>
<li>
<p>🐛 Fixes container resource leaks on start failure and during teardown (<a href="">#2879</a>, <a href="">#2887</a>)</p>
</li>
<li>
<p>🐛 Removes potential partial cache-population in case of error (<a href="">#2892</a>)</p>
</li>
<li>
<p>🐛 Raises a clear error when <code>ANDROID_API_LEVEL</code> is not an integer (<a href="">#2891</a>)</p>
</li>
<li>
<p>🐛 Replaces assert with proper exception in python-build-standalone (<a href="">#2859</a>)</p>
</li>
<li>
<p>🐛 Uses ConfigurationError when <code>package_dir</code> is outside cwd instead of a generic Exception (<a href="">#2898</a>)</p>
</li>
<li>
<p>🛠 Updates dependencies and container pins (<a href="">#2893</a>, <a href="">#2882</a>, <a href="">#2874</a>, <a href="">#2868</a>, <a href="">#2862</a>, <a href="">#2884</a>, <a href="">#2845</a>, <a href="">#2837</a>, <a href="">#2818</a>, <a href="">#2810</a>, <a href="">#2838</a>, <a href="">#2813</a>)</p>
</li>
<li>
<p>🛠 Updates Android to Python 3.13.13 and 3.14.4 (<a href="">#2821</a>)</p>
</li>
<li>
<p>🛠 Applies Pyodide-specific patches to the Emscripten toolchain installation (<a href="">#2800</a>)</p>
</li>
<li>
<p>🛠 Uses <code>python -V -V</code> for Windows build diagnostics (<a href="">#2832</a>)</p>
</li>
<li>
<p>🛠 Simplifies pinned container image lookup (<a href="">#2897</a>)</p>
</li>
<li>
<p>🛠 Minor fixups across error messages, OCI container, and options (<a href="">#2860</a>)</p>
</li>
<li>
<p>💼 Adds PEP 723 metadata for <code>bin/</code> scripts and drops the <code>bin</code> dependency group (<a href="">#2819</a>)</p>
</li>
<li>
<p>💼 Improves Azure test reliability with retries and caching (<a href="">#2890</a>)</p>
</li>
<li>
<p>💼 Fixes Windows GitLab CI test running (<a href="">#2870</a>)</p>
</li>
<li>
<p>💼 Updates CI action pins and dev dependencies (<a href="">#2902</a>, <a href="">#2867</a>, <a href="">#2851</a>, <a href="">#2843</a>, <a href="">#2826</a>, <a href="">#2823</a>, <a href="">#2820</a>, <a href="">#2807</a>)</p>
</li>
<li>
<p>💼 Adds agent and copilot setup files (<a href="">#2861</a>)</p>
</li>
<li>
<p>💼 Uses <code>if TYPE_CHECKING:</code> blocks (<a href="">#2866</a>, <a href="">#2864</a>)</p>
</li>
<li>
<p>🧪 Fixes Android tests using the <code>uv</code> frontend (<a href="">#2809</a>)</p>
</li>
<li>
<p>🧪 Fixes the update-dependencies workflow to use <code>uv</code> to run <code>nox</code> (<a href="">#2808</a>)</p>
</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<h3>v4.1.0</h3>
<p><em>12 June 2026</em></p>
<ul>
<li>✨ Updates Pyodide to the final 314.0.0 release, so Pyodide 3.14 wheels now build by default without the <code>pyodide-prerelease</code> <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code></a> flag. (<a href="">#2906</a>)</li>
<li>🐛 Raises clear errors when a build produces no wheel, instead of failing later with a confusing message (<a href="">#2909</a>)</li>
<li>🛠 Speeds up CLI startup through lazy imports on Python 3.15 (<a href="">#2797</a>)</li>
<li>📚 Adds an FAQ section on caching cibuildwheel's downloaded tools with <code>CIBW_CACHE_PATH</code> (<a href="">#2842</a>)</li>
<li>📚 Documentation improvements: clarifies which shell is used for command options, clarifies environment variable precedence, and fixes a dead Pyodide env info link (<a href="">#2904</a>, <a href="">#2905</a>, <a href="">#2911</a>)</li>
</ul>
<h3>v4.0.0</h3>
<p><em>7 June 2026</em></p>
<p>See <a href="https://github.com/henryiii"><code>@​henryiii</code></a>'s <a href="https://iscinumpy.dev/post/cibuildwheel-4-0-0/">release post</a> for more info on new features!</p>
<ul>
<li>
<p>🌟 Adds wheel auditing with <code>abi3audit</code> as a default after the repair step, with new <a href="https://cibuildwheel.pypa.io/en/stable/options/#audit-requires"><code>audit-requires</code></a> and <a href="https://cibuildwheel.pypa.io/en/stable/options/#audit-command"><code>audit-command</code></a> options (<a href="">#2805</a>)</p>
</li>
<li>
<p>🌟 Adds <code>pyemscripten</code> platform tag support (PEP 783), updates Pyodide to 314.0.0a2, and adds a <code>pyodide-eol</code> <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code></a> flag for building end-of-life Pyodide versions (<a href="">#2812</a>, <a href="">#2848</a>)</p>
</li>
<li>
<p>🌟 Sets up <code>delvewheel</code> as the default <a href="https://cibuildwheel.pypa.io/en/stable/options/#repair-wheel-command"><code>repair-wheel-command</code></a> for Windows, so extension module DLLs are now bundled automatically. Skip by setting it to empty if not needed. (<a href="">#2831</a>)</p>
</li>
<li>
<p>✨ Adds CPython 3.15 support, under the <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code> option</a> <code>cpython-prerelease</code>. This version of cibuildwheel uses 3.15.0b2. (<a href="">#2833</a>, <a href="">#2850</a>)</p>
<p><em>While CPython is in beta, the ABI can change, so your wheels might not be compatible with the final release. For this reason, we don't recommend distributing wheels until RC1, at which point 3.15 will be available in cibuildwheel without the flag.</em></p>
</li>
<li>
<p>✨ Adds CPython 3.15 support for iOS and Android (<a href="">#2857</a>, <a href="">#2858</a>)</p>
</li>
<li>
<p>✨ Adds Android improvements for building NumPy and related packages, including auditwheel support, pkg-config and Fortran configuration, and the <a href="https://cibuildwheel.pypa.io/en/stable/options/#xbuild-files"><code>xbuild-files</code></a> option (<a href="">#2695</a>)</p>
</li>
<li>
<p>✨ Adds <code>CIBUILDWHEEL_BUILD_IDENTIFIER</code> environment variable set to the current build identifier (e.g. <code>cp311-manylinux_x86_64</code>) during per-build steps (<a href="">#2872</a>)</p>
</li>
<li>
<p>✨ Adds <code>{project}</code> and <code>{package}</code> placeholders to <a href="https://cibuildwheel.pypa.io/en/stable/options/#config-settings"><code>config-settings</code></a> (<a href="">#2827</a>)</p>
</li>
<li>
<p>⚠️ Drops support for Python 3.8 (<a href="">#2686</a>)</p>
</li>
<li>
<p>⚠️ Removes the experimental CPython 3.13 free-threading builds and the <code>cpython-freethreading</code> <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code></a> option. CPython 3.14+ free-threading support remains available without the enable flag. (<a href="">#2684</a>)</p>
</li>
<li>
<p>⚠️ Drops support for Cirrus CI, which is shutting down June 1, 2026 (<a href="">#2817</a>)</p>
</li>
<li>
<p>⚠️ Drops GraalPy 3.11 (gp311) support, as agreed in <a href="">#2741</a>, and removes GraalPy 24-only workarounds (<a href="">#2895</a>)</p>
</li>
<li>
<p>🔐 Adds SHA256 verification for direct downloads of Python interpreters, virtualenv, and python-build-standalone assets (<a href="">#2873</a>)</p>
</li>
<li>
<p>🔐 Adds tarfile extraction filter for safe archive extraction (<a href="">#2856</a>)</p>
</li>
<li>
<p>🐛 Fixes <code>UV_PYTHON</code> not being set for <a href="https://cibuildwheel.pypa.io/en/stable/options/#before-build"><code>before-build</code></a> on Linux when using <code>uv</code> as the <a href="https://cibuildwheel.pypa.io/en/stable/options/#build-frontend"><code>build-frontend</code></a> (<a href="">#2830</a>)</p>
</li>
<li>
<p>🐛 Fixes detection of musl libc when downloading python-build-standalone, which previously always selected the gnu asset on musl hosts like Alpine (<a href="">#2889</a>)</p>
</li>
<li>
<p>🐛 Fixes <a href="https://cibuildwheel.pypa.io/en/stable/options/#config-settings"><code>config-settings</code></a> expansion when <code>{project}</code> or <code>{package}</code> contains spaces or backslashes (<a href="">#2886</a>)</p>
</li>
<li>
<p>🐛 Prevents deadlock when <code>linux32</code> fails and forwards platform args to the sanity check (<a href="">#2880</a>, <a href="">#2888</a>)</p>
</li>
<li>
<p>🐛 Fixes container resource leaks on start failure and during teardown (<a href="">#2879</a>, <a href="">#2887</a>)</p>
</li>
<li>
<p>🐛 Removes potential partial cache-population in case of error (<a href="">#2892</a>)</p>
</li>
<li>
<p>🐛 Raises a clear error when <code>ANDROID_API_LEVEL</code> is not an integer (<a href="">#2891</a>)</p>
</li>
<li>
<p>🐛 Replaces assert with proper exception in python-build-standalone (<a href="">#2859</a>)</p>
</li>
<li>
<p>🐛 Uses ConfigurationError when <code>package_dir</code> is outside cwd instead of a generic Exception (<a href="">#2898</a>)</p>
</li>
<li>
<p>🛠 Updates dependencies and container pins (<a href="">#2893</a>, <a href="">#2882</a>, <a href="">#2874</a>, <a href="">#2868</a>, <a href="">#2862</a>, <a href="">#2884</a>, <a href="">#2845</a>, <a href="">#2837</a>, <a href="">#2818</a>, <a href="">#2810</a>, <a href="">#2838</a>, <a href="">#2813</a>)</p>
</li>
<li>
<p>🛠 Updates Android to Python 3.13.13 and 3.14.4 (<a href="">#2821</a>)</p>
</li>
<li>
<p>🛠 Applies Pyodide-specific patches to the Emscripten toolchain installation (<a href="">#2800</a>)</p>
</li>
<li>
<p>🛠 Uses <code>python -V -V</code> for Windows build diagnostics (<a href="">#2832</a>)</p>
</li>
<li>
<p>🛠 Simplifies pinned container image lookup (<a href="">#2897</a>)</p>
</li>
<li>
<p>🛠 Minor fixups across error messages, OCI container, and options (<a href="">#2860</a>)</p>
</li>
<li>
<p>💼 Adds PEP 723 metadata for <code>bin/</code> scripts and drops the <code>bin</code> dependency group (<a href="">#2819</a>)</p>
</li>
<li>
<p>💼 Improves Azure test reliability with retries and caching (<a href="">#2890</a>)</p>
</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>2947353</code></a> Bump version: v4.1.0</li>
<li><a href=""><code>14a3c3a</code></a> Remove Travis pre-commit check</li>
<li><a href=""><code>42aa134</code></a> chore: minor cleanups and perf tweaks from code review (<a href="">#2910</a>)</li>
<li><a href=""><code>01265e5</code></a> Clarify shell used for command options (<a href="">#2904</a>)</li>
<li><a href=""><code>f4afd95</code></a> Add FAQ section on caching cibuildwheel's downloaded tools (<a href="">#2842</a>)</li>
<li><a href=""><code>6c08562</code></a> fix: faster CLI on Python 3.15 (<a href="">#2797</a>)</li>
<li><a href=""><code>4f42ee3</code></a> fix: raise clear errors when no wheel is produced (<a href="">#2909</a>)</li>
<li><a href=""><code>f3aa1be</code></a> Fix dead Pyodide env info link, remove mention of alpha ABI (<a href="">#2911</a>)</li>
<li><a href=""><code>d60fc2b</code></a> Support new graalpy asset names that include Python version. (<a href="">#2863</a>)</li>
<li><a href=""><code>55c8985</code></a> docs: clarify environment precedence (<a href="">#2905</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/v3.4.0...v4.1.0">compare view</a></li>
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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e9d2b0ddba95`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.