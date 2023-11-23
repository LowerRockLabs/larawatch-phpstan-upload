# larawatch-phpstan-upload

## IN DEVELOPMENT

Used by Larawatch to update PHPStan status for a project

## Example
```
name: run-phpstan

on: [push, pull_request]

jobs:
  test:
    runs-on: ghrun-large
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest]
        php: [8.2]
        laravel: [10]
        stability: [prefer-dist]

    name: PHPStan-PHP82-L10-preferdist-ubuntu-latest
    env:
      extensionKey: PHP82-Extensions
      extensions: dom, curl, libxml, mbstring, zip, pcntl, pdo, bcmath, soap, intl, gd, exif, iconv, imagick, fileinfo

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup cache environment
        id: extcache
        uses: shivammathur/cache-extensions@v1
        with:
          php-version: ${{ matrix.php }}
          extensions: ${{ env.extensions }}
          key: ${{ env.extensionKey }}

      - name: Cache extensions
        uses: actions/cache@v3
        with:
          path: ${{ steps.extcache.outputs.dir }}
          key: ${{ steps.extcache.outputs.key }}
          restore-keys: ${{ steps.extcache.outputs.key }}

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: ${{ matrix.php }}
          extensions: ${{ env.extensions }}
          coverage: none
          tools: none
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Setup problem matchers for PHP
        run: echo "::add-matcher::${{ runner.tool_cache }}/php.json"
          
      - name: Get composer cache directory
        id: composer-cache
        run: |
          echo "dir=$(composer config cache-files-dir)" >> $GITHUB_OUTPUT

      - uses: actions/cache@v3
        with:
          path: ${{ steps.composer-cache.outputs.dir }}
          key: PHPStan-PHP82-L10-composer-${{ hashFiles('**/composer.json') }}
          restore-keys: PHPStan-PHP82-L10-composer-${{ hashFiles('**/composer.json') }}

      - name: Add token
        run: |
          composer config github-oauth.github.com ${{ secrets.GITHUB_TOKEN }}
          
      - name: Install dependencies
        if: steps.composer-cache.outputs.cache-hit != 'true'
        run: composer require "laravel/framework:${{ matrix.laravel }}.*" --no-interaction --no-update

      - name: Update dependencies
        if: steps.composer-cache.outputs.cache-hit != 'true'
        run: composer update --${{ matrix.stability }} --no-interaction

      - name: Install nunomaduro/larastan
        run: composer require "nunomaduro/larastan:^2.6" --no-interaction

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 1 > ./phpstan-level-1.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true

      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-1.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "1" }'

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 2 > ./phpstan-level-2.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true

      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-2.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "2" }'

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 3 > ./phpstan-level-3.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true
      
      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-3.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "3" }'


      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 4 > ./phpstan-level-4.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true
      
      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-4.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "4" }'

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 5 > ./phpstan-level-5.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true
      
      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-5.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "5" }'

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 6 > ./phpstan-level-6.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true

      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-6.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "6" }'

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 7 > ./phpstan-level-7.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true

      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-7.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "7" }'

      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 8 > ./phpstan-level-8.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true

      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-8.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "8" }'


      - shell: bash
        run: |
          php ./vendor/bin/phpstan --error-format=json analyse -l 9 > ./phpstan-level-9.json
          echo "exitcode=0" >> $GITHUB_OUTPUT
        continue-on-error: true

      - uses: LowerRockLabs/larawatch-phpstan-upload@v0.2
        env:
          PROJECT_KEY: ${{ secrets.PROJECT_KEY }}
        with:
          filePath: 'phpstan-level-9.json'
          data: '{ "pwd": "${{ github.workspace }}", "project_key": "${{ secrets.PROJECT_KEY }}", "phpstan_level": "9" }'```