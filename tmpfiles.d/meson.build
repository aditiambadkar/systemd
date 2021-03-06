# SPDX-License-Identifier: LGPL-2.1-or-later

enable_tmpfiles = conf.get('ENABLE_TMPFILES') == 1

files = [['README',               ''],
         ['home.conf',            ''],
         ['journal-nocow.conf',   ''],
         ['systemd-nologin.conf', 'HAVE_PAM'],
         ['systemd-nspawn.conf',  'ENABLE_MACHINED'],
         ['systemd-tmp.conf',     ''],
         ['portables.conf',       'ENABLE_PORTABLED'],
         ['systemd-pstore.conf',  'ENABLE_PSTORE'],
         ['tmp.conf',             ''],
         ['x11.conf',             ''],
         ['legacy.conf',          'HAVE_SYSV_COMPAT'],
        ]

foreach pair : files
        if not enable_tmpfiles
                # do nothing
        elif pair[1] == '' or conf.get(pair[1]) == 1
                install_data(pair[0], install_dir : tmpfilesdir)
        else
                message('Not installing tmpfiles.d/@0@ because @1@ is @2@'
                        .format(pair[0], pair[1], conf.get(pair[1], 0)))
        endif
endforeach

in_files = ['etc.conf',
            'static-nodes-permissions.conf',
            'systemd.conf',
            'var.conf']

foreach file : in_files
        custom_target(
                # XXX: workaround for old meson. Drop when upgrading.
                'tmpfiles+' + file,
                input : file + '.in',
                output: file,
                command : [meson_render_jinja2, config_h, '@INPUT@'],
                capture : true,
                install : enable_tmpfiles,
                install_dir : tmpfilesdir)
endforeach

if enable_tmpfiles and install_sysconfdir
        meson.add_install_script(
                'sh', '-c',
                mkdir_p.format(join_paths(sysconfdir, 'tmpfiles.d')))
endif
