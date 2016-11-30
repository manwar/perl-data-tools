

# NAME

    Data::Tools provides set of basic functions for data manipulation.

# SYNOPSIS

    use Data::Tools qw( :all );  # import all functions
    use Data::Tools;             # the same as :all :) 
    use Data::Tools qw( :none ); # do not import anything, use full package names

    # --------------------------------------------------------------------------

    my $res     = file_save( $file_name, 'file content here' );
    my $content = file_load( $file_name );

    # --------------------------------------------------------------------------

    my $file_modification_time_in_seconds = file_mtime( $file_name );
    my $file_change_time_in_seconds       = file_ctime( $file_name );
    my $file_last_access_time_in_seconds  = file_atime( $file_name );
    my $file_size                         = file_size(  $file_name );

    # --------------------------------------------------------------------------
    

    my $res  = dir_path_make( '/path/to/somewhere' ); # create full path with 0700
    my $res  = dir_path_make( '/new/path', MASK => 0755 ); # ...with mask 0755
    my $path = dir_path_ensure( '/path/s/t/h' ); # ensure path exists, check+make

    # --------------------------------------------------------------------------
    

    my $hash_str = hash2str( $hash_ref ); # convert hash to string "key=value\n"
    my $hash_ref = str2hash( $hash_str ); # convert str "key-value\n" to hash
    

    my $hash_ref = url2hash( 'key1=val1&key2=val2&testing=tralala);
    # $hash_ref will be { key1 => 'val1', key2 => 'val2', testing => 'tralala' }

    my $hash_ref_with_upper_case_keys = hash_uc( $hash_ref_with_lower_case_keys );
    my $hash_ref_with_lower_case_keys = hash_lc( $hash_ref_with_upper_case_keys );

    hash_uc_ipl( $hash_ref_to_be_converted_to_upper_case_keys );
    hash_lc_ipl( $hash_ref_to_be_converted_to_lower_case_keys );
    

    # save/load hash in str_url_escaped form to/from a file
    my $res      = hash_save( $file_name, $hash_ref );
    my $hash_ref = hash_load( $file_name );

    # validate (nested) hash by example
    

    # validation example nested hash
    my $validate_hr = {
                      A => 'INT',
                      B => 'INT(-5,10)',
                      C => 'REAL',
                      D => {
                           E => 'RE:\d+[a-f]*',  # regexp match
                           F => 'REI:\d+[a-f]*', # case insensitive regexp match
                           },
                      DIR1  => '-d',   # must be existing directory
                      DIR2  => 'dir',  # must be existing directory
                      FILE1 => '-f',   # must be existing file  
                      FILE2 => 'file', # must be existing file  
                      };
    # actual nested hash to be verified if looks like the example
    my $data_hr     = {
                      A => '123',
                      B =>  '-1',
                      C =>  '1 234 567.89',
                      D => {
                           E => '123abc',
                           F => '456FFF',
                           },
                      }               
    

    my @invalid_keys = hash_validate( $data_hr, $validate_hr );
    print "YES!" if hash_validate( $data_hr, $validate_hr );

    # --------------------------------------------------------------------------
    

    my $escaped   = str_url_escape( $plain_str ); # URL-style %XX escaping
    my $plain_str = str_url_unescape( $escaped );

    my $escaped   = str_html_escape( $plain_str ); # HTML-style &name; escaping
    my $plain_str = str_html_unescape( $escaped );
    

    my $hex_str   = str_hex( $plain_str ); # HEX-style XX string escaping
    my $plain_str = str_unhex( $hex_str );

    # --------------------------------------------------------------------------
    

    # converts perl package names to file names, f.e: returns "Data/Tools.pm"
    my $perl_pkg_fn = perl_package_to_file( 'Data::Tools' );

    # --------------------------------------------------------------------------

    # calculating hex digests
    my $whirlpool_hex = wp_hex( $data );
    my $sha1_hex      = sha1_hex( $data );
    my $md5_hex       = md5_hex( $data );

    # --------------------------------------------------------------------------

    my $formatted_str = str_num_comma( 1234567.89 );  # returns "1'234'567.89"
    my $padded_str    = str_pad( 'right', -12, '*' ); # returns "right*******"
    my $str_c         = str_countable( $dc, 'day', 'days' );
                        # returns 'days' for $dc == 0
                        # returns 'day'  for $dc == 1
                        # returns 'days' for $dc >  1

    # --------------------------------------------------------------------------

    # find all *.txt files in all subdirectories starting from /usr/local
    # returned files are with full path names
    my @files = glob_tree( '/usr/local/*.txt' );

    # read directory entries names (without full paths)
    my @files_and_dirs = read_dir_entries( '/tmp/secret/dir' );

# FUNCTIONS

## hash\_validate( $data\_hr, $validate\_hr );

Return value can be either scalar or array context. In scalar context return
value is true (1) or false (0). In array context it returns list of the invalid
keys (possibly key paths like 'KEY1/KEY2/KEY3'):

    # array context
    my @invalid_keys = hash_validate( $data_hr, $validate_hr );
    

    # scalar context
    print "YES!" if hash_validate( $data_hr, $validate_hr );

# TODO

    (more docs)

# DATA::TOOLS SUB-MODULES

Data::Tools package includes several sub-modules:

    * Data::Tools::Socket (socket I/O processing, TODO: docs)
    * Data::Tools::Time   (time processing)

# REQUIRED MODULES

Data::Tools is designed to be simple, compact and self sufficient. 
However it uses some 3rd party modules:

    * Digest::Whirlpool
    * Digest::MD5
    * Digest::SHA1

# SEE ALSO

For more complex cases of nested hash validation, 
check Data::Validate::Struct module by Thomas Linden, cheers :)

# GITHUB REPOSITORY

    git@github.com:cade-vs/perl-data-tools.git
    

    git clone git://github.com/cade-vs/perl-data-tools.git
    

# AUTHOR

    Vladi Belperchinov-Shabanski "Cade"

    <cade@biscom.net> <cade@datamax.bg> <cade@cpan.org>

    http://cade.datamax.bg


# NAME

    Data::Tools::Socket provides set of socket I/O functions.

# SYNOPSIS

    use Data::Tools qw( :all );  # import all functions
    use Data::Tools;             # the same as :all :) 
    use Data::Tools qw( :none ); # do not import anything, use full package names

    # --------------------------------------------------------------------------

    my $read_res_len  = socket_read(  $socket, $data_ref, $length, $timeout );
    my $write_res_len = socket_write( $socket, $data,     $length, $timeout );
    my $write_res_len = socket_print( $socket, $data, $timeout );

    # --------------------------------------------------------------------------

    my $read_data = socket_read_message(  $socket, $timeout );
    my $write_res = socket_write_message( $socket, $data, $timeout );

    # --------------------------------------------------------------------------

# FUNCTIONS

## socket\_read(  $socket, $data\_ref, $length, $timeout )

Reads $length sized data from the $socket and store it to $data\_ref scalar 
reference.

Returns read length (can be shorter than requested $length);

$timeout is optional, it is in seconds and can be less than 1 second.

## socket\_write( $socket, $data,     $length, $timeout )

Writes $length sized data from $data scalar to the $socket.

Returns write length (can be shorter than requested $length);

$timeout is optional, it is in seconds and can be less than 1 second.

## socket\_print( $socket, $data, $timeout )

Same as socket\_write() but calculates requested length from the $data scalar.

$timeout is optional, it is in seconds and can be less than 1 second.

## socket\_read\_message(  $socket, $timeout )

Reads 32bit network-order integer, which then is used as data size to be read
from the socket (i.e. message = 32bit-integer + data ).

Returns read data or undef for message or network error.

$timeout is optional, it is in seconds and can be less than 1 second.

## socket\_write\_message( $socket, $data, $timeout )

Writes 32bit network-order integer, which is the size of the given $data to be
written to the $socket and then writes the data 
(i.e. message = 32bit-integer + data ).

Returns 1 on success or undef for message or network error.

$timeout is optional, it is in seconds and can be less than 1 second.

# TODO

    * more docs

# REQUIRED MODULES

Data::Tools::Socket uses:

    * IO::Select
    * Time::HiRes

# GITHUB REPOSITORY

    git@github.com:cade-vs/perl-data-tools.git
    

    git clone git://github.com/cade-vs/perl-data-tools.git
    

# AUTHOR

    Vladi Belperchinov-Shabanski "Cade"

    <cade@biscom.net> <cade@datamax.bg> <cade@cpan.org>

    http://cade.datamax.bg


# NAME

    Data::Tools::Time provides set of basic functions for time processing.

# SYNOPSIS

    use Data::Tools qw( :all );  # import all functions
    use Data::Tools;             # the same as :all :) 
    use Data::Tools qw( :none ); # do not import anything, use full package names

    # --------------------------------------------------------------------------

    my $time_diff_str     = unix_time_diff_in_words( $time1 - $time2 );
    my $time_diff_str_rel = unix_time_diff_in_words_relative( $time1 - $time2 );

    # --------------------------------------------------------------------------
      

    my $date_diff_str     = julian_date_diff_in_words( $date1 - $date2 );
    my $date_diff_str_rel = julian_date_diff_in_words_relative( $date1 - $date2 );

    # --------------------------------------------------------------------------

# FUNCTIONS

## unix\_time\_diff\_in\_words( $unix\_time\_diff )

Returns human-friendly text for the give time difference (in seconds).
This function returns absolute difference text, for relative 
(before/after/ago/in) see unix\_time\_diff\_in\_words\_relative().

## unix\_time\_diff\_in\_words\_relative( $unix\_time\_diff )

Same as unix\_time\_diff\_in\_words() but returns relative text
(i.e. with before/after/ago/in)

## julian\_date\_diff\_in\_words( $julian\_date\_diff );

Returns human-friendly text for the give date difference (in days).
This function returns absolute difference text, for relative 
(before/after/ago/in) see julian\_day\_diff\_in\_words\_relative().

## julian\_date\_diff\_in\_words\_relative( $julian\_date\_diff );

Same as julian\_date\_diff\_in\_words() but returns relative text
(i.e. with before/after/ago/in)

# TODO

    * support for language-dependent wording (before/ago)
    * support for user-defined thresholds (48 hours, 2 months, etc.)

# REQUIRED MODULES

Data::Tools::Time uses only:

    * Data::Tools (from the same package)

# TEXT TRANSLATION NOTES

time/date difference wording functions does not have translation functions
and return only english text. This is intentional since the goal is to keep
the translation mess away but still allow simple (yet bit strange) 
way to translate the result strings with regexp and language hash:
  

    my $time_diff_str_rel = unix_time_diff_in_words_relative( $time1 - $time2 );
    

    my %TRANS = (
                'now'       => 'sega',
                'today'     => 'dnes',
                'tomorrow'  => 'utre',
                'yesterday' => 'vchera',
                'in'        => 'sled',
                'before'    => 'predi',
                'year'      => 'godina',
                'years'     => 'godini',
                'month'     => 'mesec',
                'months'    => 'meseca',
                'day'       => 'den',
                'days'      => 'dni',
                'hour'      => 'chas',
                'hours'     => 'chasa',
                'minute'    => 'minuta',
                'minutes'   => 'minuti',
                'second'    => 'sekunda',
                'seconds'   => 'sekundi',
                );
                

    $time_diff_str_rel =~ s/([a-z]+)/$TRANS{ lc $1 } || $1/ge;

I know this is no good for longer sentences but works fine in this case.

# GITHUB REPOSITORY

    git@github.com:cade-vs/perl-data-tools.git
    

    git clone git://github.com/cade-vs/perl-data-tools.git
    

# AUTHOR

    Vladi Belperchinov-Shabanski "Cade"

    <cade@bis.bg> <cade@biscom.net> <cade@datamax.bg> <cade@cpan.org>

    http://cade.datamax.bg
